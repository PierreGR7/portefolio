<!-- components/content/Card.vue -->
<template>
  <div class="p-1 text-center" >
    <div v-html="latex"></div>
  </div>
</template>

<script setup>
const props = defineProps({
  latex: {
    type: String,
    required: true
  }
});


onMounted(() => {
  MathJax.typeset();
});

</script>