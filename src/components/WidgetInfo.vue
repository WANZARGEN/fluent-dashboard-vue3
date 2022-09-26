<script setup lang="ts">
interface WidgetInfoProps {
  name: string;
}

defineProps<WidgetInfoProps>();
</script>

<template>
  <div class="widget-info">
    <p>{{ name }}</p>
  </div>
</template>

<style scoped>
.widget-info {
  border: 1px solid ghostwhite;
  padding: 0.25rem;
  border-radius: 0.5rem;
  margin: 0.25rem 0.25rem;
  cursor: grab;
}
</style>
