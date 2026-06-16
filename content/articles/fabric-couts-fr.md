---
title: Microsoft Fabric — Garder ses coûts sous contrôle
description: Retour d'expérience sur le monitoring et la réduction de la facture Microsoft Fabric : l'app Metrics, le lissage, le throttling et les leviers qui comptent.
authors:
  - name: Pierre Graef
    to: https://www.linkedin.com/in/pierre-graef
    avatar:
      src: /img/profil.png
image:
  src: /img/article_fabric/cover.svg
date: 2026-06-16T01:00:00.000Z
lang: fr
translationPath: /articles/fabric-couts
---

Chez nous, les coûts Fabric sont devenus un sujet le jour où la facture Azure a doublé d'un mois sur l'autre. Au début personne ne regarde : ça tourne, les rapports se rafraîchissent, tout va bien. Puis quelqu'un ouvre la facture, et c'est à moi d'expliquer pourquoi.

Le point qui change tout, c'est qu'on ne paie pas à la requête. On paie une **capacité**, un pool de calcul partagé dans lequel tout vient piocher : notebooks Spark, pipelines, entrepôt, modèles sémantiques, rendu Power BI. Résultat, la facture ne dit jamais qui a consommé quoi. Un pipeline qui part en boucle ou un modèle rafraîchi toutes les quinze minutes mange exactement la même capacité que les dashboards regardés chaque matin, et ça passe inaperçu un moment.

## L'app Metrics avant tout

On ne coupe pas ce qu'on ne voit pas. La première chose à installer, c'est l'app **Microsoft Fabric Capacity Metrics**. On la pointe sur la capacité et on a l'usage des CU dans le temps, les throttlings, et surtout le classement des éléments les plus consommateurs. C'est là que j'ai trouvé le coupable chez nous : un rafraîchissement planifié sur un modèle que plus personne n'ouvrait, qui tournait depuis des mois. Au-dessus, je pose un budget avec alertes dans Azure Cost Management pour être prévenu avant la réunion budget, pas pendant.

## Lissage et throttling

Fabric lisse les pics. Les opérations interactives sont lissées sur quelques minutes, celles d'arrière-plan (pipelines, rafraîchissements, notebooks) sur 24 heures. Quand l'usage projeté monte trop, il throttle : il retarde l'interactif, puis le rejette, puis rejette l'arrière-plan. La première fois, mon réflexe a été de demander un SKU plus gros. C'était la mauvaise réponse : on avait empilé tous les jobs lourds à 2 h du matin. On a étalé les planifications et le throttling a disparu sans dépenser plus.

## Ce qui fait baisser la note

Le plus gros gain reste de suspendre les capacités hors prod. Une capacité de dev allumée la nuit et le week-end, c'est de l'argent perdu : en pay-as-you-go, on automatise pause et reprise sur les heures ouvrées et on coupe la facture hors-prod de plus de moitié. Ensuite, réserver ce qui tourne vraiment en continu (environ 40 % d'économie), tout en gardant les charges en pics en pay-as-you-go pour pouvoir les mettre en pause. Le reste, c'est de l'hygiène : dimensionner sur le vrai pic mesuré, étaler les rafraîchissements, et soigner Spark, là où part le plus de CU pour rien (pools bien dimensionnés, moins de petits fichiers, V-Order et Direct Lake quand le modèle s'y prête).

Le bon ordre, c'est de commencer par le structurel : suspendre ce qui est inactif, réserver ce qui est stable, étaler ce qui est lourd. La micro-optimisation vient après, une fois que l'app Metrics montre où regarder.
