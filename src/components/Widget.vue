<script setup lang="ts">
interface WidgetProps {
  id: string;
  name: string;
  contents: string;
  minWidth: number;
  maxWidth: number;
}

defineProps<WidgetProps>();
</script>

<template>
  <div class="widget">
    <p class="title">{{ name }}</p>
    <div class="widget-contents">{{ contents }}</div>
  </div>
</template>

<style scoped>
.widget {
  border: 1px solid aquamarine;
  padding: 0.5rem;
  border-radius: 0.5rem;
  margin: 8px 8px;
  min-height: 150px;
}
.title {
  font-weight: bold;
  font-size: 1.5rem;
  padding-bottom: 1rem;
}
</style>
