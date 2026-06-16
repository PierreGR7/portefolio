---
title: Microsoft Fabric — Garder ses coûts de capacité sous contrôle
description: Retour d'expérience sur le monitoring et la réduction de la facture Microsoft Fabric — l'app Metrics, le lissage, le throttling, et les leviers qui font vraiment bouger la note.
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

Au boulot, la question des coûts Fabric est revenue à peu près le mois où on a basculé une vraie charge de prod dessus. Au début personne ne regarde : ça marche, les rapports tournent, tout le monde est content. Et puis quelqu'un ouvre la facture Azure et demande pourquoi elle a doublé. Là, ça devient mon problème.

Le truc avec Fabric, c'est qu'on ne paie pas à la requête. On paie une **capacité** — un pool de calcul partagé dans lequel tout vient piocher : les notebooks Spark, les pipelines, l'entrepôt, les modèles sémantiques, le rendu Power BI. C'est pratique, mais ça veut dire que la facture ne dit jamais *qui* a consommé quoi. Et quand un pipeline part en vrille ou qu'un modèle se rafraîchit toutes les quinze minutes, il mange exactement la même capacité que les dashboards que la direction regarde le matin. Personne ne s'en rend compte tout de suite. C'est ça le piège.

## D'abord comprendre ce qu'on paie

On facture en **unités de capacité (CU)**. On prend un SKU — F2, F4, F8, et ainsi de suite jusqu'à des tailles que vous n'achèterez probablement jamais — et le chiffre correspond grosso modo aux CU par seconde dont on dispose. Tout passe par là.

Deux choses ont vraiment changé ma façon de raisonner sur le coût. La première, c'est qu'une capacité en **pay-as-you-go** se met **en pause**. En pause, elle ne facture plus de calcul. La deuxième, c'est la **réservation** sur un an : environ 40 % moins cher, mais ça tourne le compteur en continu, qu'on s'en serve ou pas. Tout l'arbitrage est là, et j'y reviens.

Il y a aussi un piège de licence que j'ai mis un moment à intégrer : à partir de **F64**, la consultation de contenu Power BI est incluse pour les lecteurs. En dessous, chaque personne qui ouvre un rapport a besoin d'une licence Pro. Si vous avez beaucoup de lecteurs, faites le calcul avant de partir sur le plus petit SKU « pour économiser » — il arrive qu'un SKU plus gros revienne moins cher une fois les licences Pro déduites. Ça paraît contre-intuitif, c'est pourtant arrivé chez nous.

Et le **stockage** OneLake se facture à part, au Go par mois. C'est en général une petite ligne, mais le mirroring et les fichiers qui traînent finissent par se voir.

## Le seul réflexe à prendre : l'app Metrics

On ne coupe pas ce qu'on ne voit pas, et au début je coupais à l'aveugle. Le jour où j'ai installé l'**app Microsoft Fabric Capacity Metrics**, j'ai compris la moitié du problème en dix minutes. On la pointe sur la capacité et on a l'usage des CU dans le temps, les throttlings, les surcharges — et surtout le classement des éléments les plus consommateurs. C'est dans ce classement que j'ai trouvé le coupable chez nous : un rafraîchissement programmé sur un modèle que plus personne ne regardait, qui tournait toutes les quinze minutes depuis des mois.

Au-dessus, je mets un **budget avec alertes** dans Azure Cost Management et je tague les capacités par environnement. Comme ça la dépense est rattachée à quelque chose, et l'alerte tombe avant la réunion budget, pas pendant.

Si vous ne devez retenir qu'une chose : installez l'app et regardez ce qui consomme. La réponse est presque toujours surprenante.

## Lissage et throttling, ou pourquoi la facture est bizarre

Fabric ne facture pas les pics bêtement. Il les **lisse**. Les opérations interactives (requêtes, clics dans un rapport) sont lissées sur quelques minutes ; les opérations d'arrière-plan (pipelines, rafraîchissements, notebooks) le sont sur **24 heures**. Par-dessus, le *bursting* autorise un gros job à dépasser temporairement la capacité pour finir plus vite, quitte à « rembourser » ensuite via le lissage.

Tout va bien jusqu'au moment où on sur-engage. Quand l'usage futur projeté grimpe trop, Fabric **throttle** par paliers : il retarde d'abord les requêtes interactives, puis les rejette, et finit par rejeter les jobs d'arrière-plan. La première fois que ça m'est tombé dessus, mon réflexe a été de demander un SKU plus gros. C'était la mauvaise réponse. Le vrai problème, c'est qu'on avait empilé tous les jobs lourds à 2 h du matin, et toute cette dette d'arrière-plan se lissait dans la journée et étranglait l'interactif. On a étalé les planifications sur la nuit, et le throttling a disparu sans dépenser un centime de plus.

## Ce qui fait vraiment baisser la note

À peu près dans l'ordre où ça paie, d'après ce que j'ai vu :

Suspendre les capacités hors prod, c'est de loin le plus gros gain et le plus bête à louper. Une capacité de dev ou de test allumée la nuit et le week-end, c'est de l'argent jeté par la fenêtre. En pay-as-you-go, on automatise pause et reprise sur les heures ouvrées et on coupe la facture hors-prod de plus de moitié sans rien casser.

Ensuite, réserver ce qui tourne vraiment en continu. Si une capacité de prod ne s'arrête jamais, les ~40 % de la réservation sont gratuits, autant les prendre. En revanche je laisse en pay-as-you-go tout ce qui est en pics ou à temps partiel — pour garder le droit de mettre en pause, qui vaut souvent plus que la remise.

Le reste, c'est de l'hygiène. Dimensionner sur le vrai pic mesuré, avec une marge, et pas sur un chiffre lancé au démarrage du projet — puis revérifier tous les trimestres, parce que les charges dérivent. Étaler les rafraîchissements et questionner chaque planif (ce modèle a-t-il *vraiment* besoin d'un rafraîchissement au quart d'heure ?). Et surtout, soigner Spark, parce que c'est là que part le plus de CU pour rien : pools bien dimensionnés, pas de problème de petits fichiers (moins de fichiers Parquet mais plus gros), V-Order pour les tables Direct Lake, Native Execution Engine quand il s'applique. Un job bien réglé coûte une fraction d'un job écrit à la va-vite. Quand le modèle s'y prête, je préfère le Direct Lake à l'import + rafraîchissement fréquent — on arrête de payer pour recharger des données qui vivent déjà dans OneLake. Et je pose un garde-fou de surge protection pour qu'un seul job pourri ne puisse pas affamer tout le monde.

## Par où commencer

Surtout pas par la micro-optimisation. Les gros gains, les rapides, sont structurels : on suspend ce qui est inactif, on réserve ce qui est stable, on étale ce qui est lourd. Le reste vient après. Installez l'app Metrics en premier pour que chaque décision soit guidée par ce que fait *réellement* la capacité et pas par une intuition — et revérifiez après chaque changement. Dans un pool partagé, l'effet d'un correctif se lit exactement là où vous pouvez le mesurer, et c'est franchement satisfaisant de voir la courbe redescendre.
