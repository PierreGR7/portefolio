---
title: Opus 4.8 — Savoir ce qu'il ne sait pas
description: Un court avis sur ce qui change vraiment avec Opus 4.8 — moins de conclusions forcées, plus d'honnêteté sur ses propres limites.
authors:
  - name: Pierre Graef
    to: https://www.linkedin.com/in/pierre-graef
    avatar:
      src: /img/profil.png
image:
  src: /img/article_opus/cover.svg
date: 2026-06-01T01:00:00.000Z
lang: fr
translationPath: /articles/opus-4-8
---

J'utilise beaucoup de LLM comme outil quotidien pour mon travail sur la donnée. La plupart partagent le même défaut agaçant : quand ils n'ont pas de quoi répondre, ils répondent quand même. Ils arrondissent une pensée à moitié formée en une conclusion sûre d'elle, et on ne découvre l'erreur qu'après qu'elle nous a déjà coûté une heure.

Ce qui ressort avec **Opus 4.8**, c'est le réflexe inverse. Quand le problème est réellement sous-spécifié, il a tendance à *le dire* plutôt qu'à masquer le trou.

## Ne pas forcer une conclusion

Posez une question ambiguë et les modèles précédents vous donnaient quand même une réponse nette et tranchée — le genre qui se lit bien et s'effondre au contact du réel. Opus 4.8 accepte plus volontiers de s'arrêter à « voici ce que je peux dire, voici ce que je ne peux pas, et voici ce qui changerait la réponse ».

Ça paraît anodin. En pratique, c'est la différence entre un outil qui *a l'air* utile et un outil qui l'est vraiment. Un « je n'ai pas assez d'éléments pour trancher » clair vaut mieux qu'un virage assuré dans le mur.

## Plus lucide sur ses propres manques

L'autre évolution, c'est la conscience de ses erreurs. Il signale plus vite l'étape bancale de son raisonnement, marque une hypothèse comme une hypothèse, et pointe la partie de sa réponse la plus susceptible d'être fausse — au lieu de tout présenter avec la même certitude plate.

Il n'est pas parfait sur ce point. Il dérape encore, affirme encore parfois des choses qu'il ne devrait pas. Mais la *direction* est la bonne : moins de fausse assurance bien polie, plus d'incertitude honnête.

## Pourquoi ça compte pour la donnée

Dans un pipeline, une réponse fausse mais sûre d'elle est la plus coûteuse — elle passe la revue, part en prod, et casse en aval. Un modèle qui expose ses propres doutes colle à la façon dont pensent déjà les bons ingénieurs : faire confiance, mais vérifier, et savoir quelles parties vérifier en premier.

Donc pas de grand verdict ici — c'est de circonstance. Je ne prétendrai pas qu'Opus 4.8 a « réglé » la fiabilité. Je dirai seulement qu'il donne l'impression d'un modèle un peu plus à l'aise avec le fait de ne pas savoir, et c'est une qualité que je prends sans hésiter face à la fausse confiance.
