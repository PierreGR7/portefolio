---
title: Microsoft Fabric — Keeping Capacity Costs Under Control
description: Notes from the field on monitoring and cutting the Microsoft Fabric bill: the Metrics app, smoothing, throttling and the levers that matter.
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

At work, Fabric costs became a topic the month the Azure bill doubled. Nobody looks at first: it runs, the reports refresh, everything is fine. Then someone opens the bill, and it falls on me to explain why.

The thing that changes everything is that you don't pay per query. You pay for a **capacity**, a shared pool of compute that everything draws from: Spark notebooks, pipelines, the warehouse, semantic models, Power BI rendering. So the bill never tells you who consumed what. A pipeline stuck in a loop or a model refreshing every fifteen minutes eats the exact same capacity as the dashboards people open every morning, and it goes unnoticed for a while.

## The Metrics app first

You can't cut what you can't see. The first thing to install is the **Microsoft Fabric Capacity Metrics** app. Point it at the capacity and you get CU usage over time, throttling events, and, most usefully, a ranking of the items consuming the most. That's where I found our culprit: a scheduled refresh on a model nobody opened anymore, running for months. On top of that I set a budget with alerts in Azure Cost Management, so I hear about it before the budget meeting, not during it.

## Smoothing and throttling

Fabric smooths spikes. Interactive operations are smoothed over a few minutes, background ones (pipelines, refreshes, notebooks) over 24 hours. When projected usage climbs too high, it throttles: it delays interactive requests, then rejects them, then rejects background jobs. The first time, my instinct was to ask for a bigger SKU. Wrong call: we had stacked every heavy job at 2 a.m. We spread the schedules out and the throttling disappeared without spending more.

## What actually brings the bill down

The biggest win is still pausing non-production capacities. A dev capacity left on overnight and through the weekend is money lost: on pay-as-you-go, automate pause and resume around working hours and you cut the non-prod bill by more than half. Next, reserve what genuinely runs around the clock (about 40% off), while keeping bursty workloads on pay-as-you-go so you can still pause them. The rest is hygiene: size to real measured peak, spread refreshes out, and tune Spark, where most CU leaks away (right-sized pools, fewer small files, V-Order and Direct Lake where the model allows it).

The right order is to start with the structural part: pause what's idle, reserve what's steady, spread what's heavy. Micro-optimisation comes after, once the Metrics app shows you where to look.
