<template>
  <div class="chart-container">
    <div class="buttons-container">
      <button @click="centerToday">Aktuell</button>
      <button @click="goLeft">zurück</button>
      <button @click="goRight">vor</button>
    </div>
  <div class="gantt-chart">
    <div class="gantt-table-container">
      <GanttTable :teamData="teamData"  />
    </div>
    <div class="gantt-diagram-container">
      <GanttDiagram :teamData="teamData"  ref="ganttDiagram"/>
    </div>
  </div>
</div>
</template>

<script setup>
import {ref} from "vue";
import GanttTable from "./GanttTable.vue";
import GanttDiagram from "./GanttDiagram.vue";

const props = defineProps({
  teamData: Array,
  required: true,
});

const ganttDiagram = ref(null);

const centerToday = () => {
  ganttDiagram.value.centerToday();
};
const goLeft = () => {
  ganttDiagram.value.goLeft();
};
const goRight = () => {
  ganttDiagram.value.goRight();
};


</script>

<style scoped>
.chart-container {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}

.buttons-container {
  max-width: 400px;
  display: flex;
  justify-content: space-between;
}

.buttons-container button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0.5rem;
  background-color: #bedd2c;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  margin: 0.5rem 0.5rem 0.5rem 0rem;
  min-width: 100px;
}

.gantt-chart {
  display: flex;
  width: 80vw;
  font-family: Arial, sans-serif;
  font-size: 1rem;
  border-collapse: collapse;
  border-radius: 1rem;
  overflow-x: hidden;

}
.gantt-table-container {
  width: 30%;
  border-collapse: collapse;
}
.gantt-diagram-container {
  width: 70%;
  border-collapse: collapse;
}
</style>
