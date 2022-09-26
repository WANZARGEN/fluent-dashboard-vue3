<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs, watch } from "vue";
import Widget from "../components/Widget.vue";
import WidgetInfo from "../components/WidgetInfo.vue";
import { faker } from "@faker-js/faker";
import Draggable from "vue3-draggable";

export default defineComponent({
  components: {
    Widget,
    WidgetInfo,
    Draggable,
  },
  setup() {
    const WIDTHS = { min: 250, max: 580 };
    const state = reactive({
      widgets: [] as Widget[],
      contextKey: faker.datatype.uuid(),
      layoutContainerRef: null as null | HTMLElement,
    });
    const reloadWidgets = () => {
      state.widgets = faker.datatype.array(10).map(() => {
        const minWidth = faker.datatype.number(WIDTHS);
        const maxWidth = faker.datatype.number({ ...WIDTHS, min: minWidth });
        return {
          id: faker.datatype.uuid(),
          name: faker.random.word(),
          contents: faker.lorem.sentences(
            faker.datatype.number({ min: 3, max: 20 })
          ),
          minWidth,
          maxWidth,
        };
      });
    };

    const PAD = 16;
    const resizeWidgets = () => {
      console.debug("resize widgets");
      const layoutContainerRef = state.layoutContainerRef;
      if (!layoutContainerRef) return;

      let containerWidth = 0;
      const { width = containerWidth } =
        layoutContainerRef.getBoundingClientRect();
      let accumulatedMin = 0;
      let accumulatedMax = 0;
      let rows = [];
      let lastWidgetIdx: number | undefined;
      state.widgets.forEach((widget, widgetIdx) => {
        const { minWidth, maxWidth } = widget;
        accumulatedMin += minWidth + PAD;
        accumulatedMax += maxWidth + PAD;
        if (accumulatedMin > containerWidth) {
          accumulatedMin = 0;
          accumulatedMax = 0;
          if (lastWidgetIdx !== undefined) {
            const row = state.widgets.slice(lastWidgetIdx, widgetIdx + 1);
            let sum = 0;
            row.forEach((rowWidget) => {
              sum += rowWidget.maxWidth;
            });
            if (sum < containerWidth) containerWidth = sum;
            rows.push(row);
          }
        }
      });
    };

    watch(
      () => state.widgets,
      () => {
        state.contextKey = faker.datatype.uuid();
      }
    );

    onMounted(() => {
      reloadWidgets();
    });

    return {
      ...toRefs(state),
      reloadWidgets,
    };
  },
});
</script>

<template>
  <div class="dashboard-container">
    <button @click="reloadWidgets">Reload</button>
    <div ref="layoutContainerRef" class="dashboard-wrapper">
      <div>
        <div class="dashboard-layout">
          <Widget
            v-for="widget in widgets"
            v-bind="widget"
            :key="widget.id"
            class="widget-card"
          />
        </div>
      </div>
      <div>
        <div class="widget-order-container">
          <Draggable :key="contextKey" v-model="widgets">
            <template #item="{ item }">
              <WidgetInfo
                :name="item.name"
                :key="item.id"
                class="widget-card"
              />
            </template>
          </Draggable>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dashboard-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.dashboard-wrapper {
  display: flex;
}
.dashboard-layout {
  display: flex;
  flex-wrap: wrap;
  justify-content: start;
  width: 100%;
  flex-grow: 1;
}
button {
  margin-bottom: 1rem;
  cursor: pointer;
  padding: 0.5rem 1rem;
}
.dashboard-layout > div {
  box-sizing: border-box;
  display: flex;
  flex-wrap: wrap;
  position: relative;
  width: 100%;
  justify-content: left;
}
</style>
