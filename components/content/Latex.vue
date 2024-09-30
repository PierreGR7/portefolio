<!-- components/content/Card.vue -->
<template>
  <span :class="{ 'mx-1': latex.startsWith('\\(') }" v-html="latex"></span>
</template>

<script setup>
const props = defineProps({
  latex: {
    type: String,
    required: true
  }
});




</script>
