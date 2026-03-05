<script setup lang="ts">
defineProps<{
  title: string
}>()
</script>

<template>
  <h2 class="section-title font-display">
    {{ title }}
  </h2>
</template>

<style scoped lang="scss">
.section-title {
  font-size: clamp(1.75rem, 5vw, 65px);
  font-weight: 200;
  color: white;
  margin-bottom: clamp(1rem, 2vw, 2rem);
  text-transform: uppercase;
}
</style>
