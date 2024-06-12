<script setup>
import HelloWorld from './components/HelloWorld.vue'
import GanttChart from './components/GanttChart.vue'
import GanttChart2 from './components/GanttChart2.vue'
import GanttChart3 from './components/ganttChart3/GanttChart3.vue'
import { ref } from 'vue';

const teamData = ref([
      {
        name: 'Max',
        totalDays: 30,
        takenDays: 10,
        plannedDays: 5,
        leftoverDays: 15,
        vacations: [
          { startDate: '2024-06-01', endDate: '2024-06-010', status: 'approved' },
          { startDate: '2024-07-01', endDate: '2024-07-03', status: 'pending' }
        ]
      },
      {
        name: 'John',
        totalDays: 30,
        takenDays: 10,
        plannedDays: 5,
        leftoverDays: 15,
        vacations: [
          { startDate: '2024-08-01', endDate: '2024-08-05', status: 'approved' },
          { startDate: '2024-07-01', endDate: '2024-07-03', status: 'cancelled' }
        ]
      },
      {
        name: 'Jane',
        totalDays: 30,
        takenDays: 10,
        plannedDays: 5,
        leftoverDays: 15,
        vacations: [
          { startDate: '2024-05-10', endDate: '2024-05-15', status: 'taken' },
          { startDate: '2024-07-01', endDate: '2024-07-03', status: 'pending' }
        ]
      },
      {
        name: 'Doe',
        totalDays: 30,
        takenDays: 10,
        plannedDays: 5,
        leftoverDays: 15,
        vacations: [
        ]
      },
      {
        name: 'Berta',
        totalDays: 30,
        takenDays: 10,
        plannedDays: 5,
        leftoverDays: 15,
        vacations: [
        ]
      }
    ])
  
  
</script>

<template>
  <div>
  <!-- <HelloWorld msg="Nice Chart" /> -->
  <!-- <GanttChart :teamData/> -->
  <!-- <GanttChart2 /> -->
  <GanttChart3 :teamData="teamData" />
  </div>
</template>

<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}


</style>
