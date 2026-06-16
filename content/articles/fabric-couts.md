---
title: Microsoft Fabric — Keeping Capacity Costs Under Control
description: Notes from the field on monitoring and cutting the Microsoft Fabric bill — the Metrics app, smoothing, throttling, and the levers that actually move the number.
authors:
  - name: Pierre Graef
    to: https://www.linkedin.com/in/pierre-graef
    avatar:
      src: /img/profil.png
image:
  src: /img/article_fabric/cover.svg
date: 2026-06-16T01:00:00.000Z
lang: en
translationPath: /articles/fabric-couts-fr
---

At work, Fabric costs became a topic roughly the month we moved a real production workload onto it. Nobody looks at first — it runs, the reports refresh, everyone's happy. Then someone opens the Azure bill and asks why it doubled. That's when it becomes my problem.

The thing with Fabric is that you don't pay per query. You pay for a **capacity** — a shared pool of compute that everything draws from: Spark notebooks, pipelines, the warehouse, semantic models, Power BI rendering. Convenient, but it means the bill never tells you *who* spent what. And when a pipeline goes sideways or a model refreshes every fifteen minutes, it's eating the exact same capacity as the dashboards management opens every morning. Nobody notices right away. That's the trap.

## Understand what you're paying for first

You're billed in **Capacity Units (CU)**. You pick a SKU — F2, F4, F8, and up through sizes you'll probably never buy — and the number is roughly the CU per second you get. Everything goes through it.

Two things genuinely changed how I think about the cost. First, a **pay-as-you-go** capacity can be **paused**, and a paused capacity bills no compute at all. Second, a one-year **reservation** runs about 40% cheaper but keeps the meter running whether you use it or not. The whole trade-off lives between those two, and I'll come back to it.

There's also a licensing gotcha that took me a while to internalise: at **F64 and above**, Power BI content viewing is included for consumers. Below that, everyone who opens a report needs a Pro license. If you have a lot of viewers, do the math before reaching for the smallest SKU "to save money" — sometimes a bigger SKU is actually cheaper once you net out the Pro licenses. Counter-intuitive, but it's happened to us.

And OneLake **storage** is billed separately, per GB per month. Usually a small line, but mirroring and stray files do add up over time.

## The one habit to build: the Metrics app

You can't cut what you can't see, and early on I was cutting blind. The day I installed the **Microsoft Fabric Capacity Metrics app**, half the problem became obvious in ten minutes. Point it at the capacity and you get CU usage over time, throttling events, overloads — and, most useful, a ranking of the items consuming the most. That ranking is where I found our culprit: a scheduled refresh on a model nobody opened anymore, running every fifteen minutes for months.

On top of that I set a **budget with alerts** in Azure Cost Management and tag capacities by environment, so spend is attributable and the alert lands *before* the budget meeting, not during it.

If you take one thing away: install the app and look at what's consuming. The answer is almost always surprising.

## Smoothing and throttling, or why the bill looks weird

Fabric doesn't bill spikes naively. It **smooths** them. Interactive operations (queries, clicking around a report) are smoothed over a few minutes; background operations (pipelines, refreshes, notebooks) are smoothed over **24 hours**. On top of that, *bursting* lets a heavy job temporarily exceed your capacity to finish faster, then pays it back through smoothing.

This is fine until you overcommit. When projected future usage climbs too high, Fabric **throttles** in stages: it delays interactive requests, then rejects them, and finally rejects background jobs. The first time this hit me, my instinct was to ask for a bigger SKU. Wrong answer. The real problem was that we'd stacked every heavy job at 2 a.m., and all that background debt smoothed into the day and choked the interactive side. We spread the schedules across the night and the throttling vanished — without spending a cent more.

## What actually brings the number down

Roughly in the order it pays off, from what I've seen:

Pausing non-production capacities is by far the biggest win and the dumbest one to miss. A dev or test capacity left on overnight and through the weekend is money out the window. On pay-as-you-go, automate pause and resume around working hours and you cut the non-prod bill by more than half without breaking anything.

Next, reserve what genuinely runs around the clock. If a prod capacity never stops, the ~40% reservation discount is free money, so take it. But I keep anything bursty or part-time on pay-as-you-go — the right to pause is usually worth more than the discount.

The rest is hygiene. Size to real measured peak with headroom, not to a number someone guessed at kickoff, and re-check it every quarter because workloads drift. Spread refreshes out and question every schedule (does that model *really* need a 15-minute refresh?). And above all, tune Spark, because that's where most CU leaks away: right-sized pools, no small-files problem (fewer, larger Parquet files), V-Order on for Direct Lake tables, Native Execution Engine where it applies. A well-tuned job costs a fraction of a sloppy one. Where the model allows it, I prefer Direct Lake over import + frequent refresh — you stop paying to reload data that already lives in OneLake. And I set a surge-protection guardrail so one bad job can't starve everyone else.

## Where to start

Definitely not with micro-optimisation. The big, fast wins are structural: pause what's idle, reserve what's steady, spread what's heavy. The rest comes later. Install the Metrics app first so every decision is driven by what the capacity is *actually* doing rather than a hunch — and re-check after each change. In a shared pool, the effect of a fix shows up exactly where you can measure it, and watching that curve come back down is honestly pretty satisfying.
