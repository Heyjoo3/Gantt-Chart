<template>
  <div class="gantt-chart">
    <div class="gantt-table-container">
      <GanttTable :teamData="teamData" />
    </div>
    <div class="gantt-diagram-container">
      <GanttDiagram :teamData="teamData" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import GanttTable from "./GanttTable.vue";
import GanttDiagram from "./GanttDiagram.vue";

const props = defineProps({
  teamData: Array,
  required: true,
});
</script>

<style scoped>
.gantt-chart {
  display: flex;
  border: 1px solid #ddd;
  width: 80vw;
  font-family: Arial, sans-serif;
  font-size: 1rem;
}

.gantt-table-container {
  width: 30%;
}

.gantt-diagram-container {
  width: 70%;
}
</style>
