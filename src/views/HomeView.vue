<script setup lang="ts">
import Widget from "../components/Widget.vue";
import { faker } from "@faker-js/faker";
import Draggable from "vue3-draggable";

const widgets = faker.datatype.array(10).map(() => ({
  id: faker.datatype.uuid(),
  name: faker.random.word(),
}));
</script>

<template>
  <main>
    <div class="dashboard-layout">
      <div class="card-list-wrapper">
        <Draggable v-model="widgets">
          <template #item="{ item }">
            <Widget :name="item.name" :key="item.id" class="widget-card" />
          </template>
        </Draggable>
      </div>
    </div>
  </main>
</template>

<style scoped>
.dashboard-layout {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin: auto;
  .card-list-wrapper {
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
    position: relative;
    width: 100%;
    justify-content: left;
  }
}
</style>
