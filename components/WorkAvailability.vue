<script setup lang="ts">
defineProps({
  availability: Boolean,
  email: String
})
</script>

<template>
  <UButton v-if="availability" color="gray" class="relative" :to="'mailto:' + email"
  >
    Available
    <span class="relative flex h-3 w-3">
            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-500 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-3 w-3 bg-green-500"></span>
          </span>
  </UButton>
  <UButton v-else color="gray" class="relative" :to="'mailto:' + email"
  >
    Not Available
    <span class="relative flex h-3 w-3">
            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-red-500 opacity-75"></span>
            <span class="relative inline-flex rounded-full h-3 w-3 bg-red-500"></span>
          </span>
  </UButton>
</template>

<style scoped>

</style>