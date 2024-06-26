<template>
  <div class="chart-container">
    <div class="gantt-header">
      <h1>Team 1</h1>
      <div class="buttons-container">
        <button @click="centerToday">Heute</button>
        <button @click="goLeft">zurück</button>
        <button @click="goRight">vor</button>
      </div>
    </div>
    <div class="gantt-chart" @mousemove="onMouseMove" @mouseup="onMouseUp">
      <div
        class="gantt-table-container"
        :style="{ flexBasis: tableWidth + 'px' }"
      >
        <GanttTable :teamData="teamData" />
      </div>
      <Splitter @drag-start="onDragStart" />
      <div class="gantt-diagram-container">
        <GanttDiagram :teamData="teamData" ref="ganttDiagram" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import GanttTable from './GanttTable.vue';
import GanttDiagram from './GanttDiagram.vue';
import Splitter from './Splitter.vue';

const props = defineProps({
  teamData: {
    type: Array,
    required: true,
  },
});

const ganttDiagram = ref(null);
const isDragging = ref(false);
const tableWidth = ref(300); // Initial width of the table container

const centerToday = () => {
  ganttDiagram.value.centerToday();
};

const goLeft = () => {
  ganttDiagram.value.goLeft();
};

const goRight = () => {
  ganttDiagram.value.goRight();
};

const onDragStart = (e) => {
  isDragging.value = true;
};

const onMouseMove = (e) => {
  if (isDragging.value) {
    const newWidth = e.clientX;
    if (newWidth >= 100) { // Ensure minimum width of 100px
      tableWidth.value = newWidth;
    }
  }
};

const onMouseUp = () => {
  isDragging.value = false;
};
</script>

<style scoped>
.chart-container {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  font-family: Arial, sans-serif;
  height: 100vh; /* Full height of the viewport */
}

.gantt-header h1 {
  color: #ddd;
}

.gantt-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 80vw;
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
  font-size: 1rem;
  border-collapse: collapse;
  border-radius: 1rem;
  overflow-x: hidden;
  /* height: calc(100vh - 150px); Adjust height based on header size */
}

.gantt-table-container {
  border-collapse: collapse;
  flex-basis: 30%;
  flex-grow: 0;
  flex-shrink: 0;
  min-width: 100px; /* Minimum width of the table container */
  overflow-y: auto; /* Enable vertical scrolling */
}

.gantt-diagram-container {
  flex-grow: 1;
  border-collapse: collapse;
  overflow-y: auto; /* Enable vertical scrolling */
}
</style>
