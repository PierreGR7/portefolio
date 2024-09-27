<template>
  <div>
    <NuxtLoadingIndicator />
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>

  </div>
</template>


<script setup>
useHead({
  script: [
    { src: 'https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js', async: true, id: 'MathJax-script' }
  ]
})
</script>