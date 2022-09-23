<script setup lang="ts">
defineProps<{
  name: string;
}>();
</script>

<template>
  <div class="widget">
    <p>Widget - {{ name }}</p>
    <div class="widget-contents"></div>
  </div>
</template>

<style scoped>
.widget {
  border: 1px solid aquamarine;
  padding: 0.5rem;
  border-radius: 0.5rem;
  margin: 0.5rem 0;
}
</style>
