<template>
    <div class="gantt-container">
      <div class="gantt-header">
        <table class="gantt-table">
          <thead>
            <tr>
              <th>Name</th>
              <th>Total Days</th>
              <th>Taken Days</th>
              <th>Planned Days</th>
              <th>Leftover Days</th>
              <th v-for="day in daysInYears" :key="day">{{ formatDate(day) }}</th>
            </tr>
          </thead>
        </table>
      </div>
      <div class="gantt-body">
        <table class="gantt-table">
          <tbody>
            <tr v-for="member in teamData" :key="member.name">
              <td class="fixed-column">{{ member.name }}</td>
              <td class="fixed-column">{{ member.totalDays }}</td>
              <td class="fixed-column">{{ member.takenDays }}</td>
              <td class="fixed-column">{{ member.plannedDays }}</td>
              <td class="fixed-column">{{ member.leftoverDays }}</td>
              <td v-for="day in daysInYears" :key="day" class="gantt-cell">
                <div v-for="vacation in member.vacations" :key="vacation.startDate" 
                     :class="getVacationClass(vacation, day)"
                     :style="getVacationStyle(vacation, day)">
                  &nbsp;
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>
  

<script setup>
import { computed, defineComponent } from 'vue';

const props = defineProps({
  teamData: Array,
  required: true
})
const daysInYears = computed(() => {
      const days = [];
      const currentDate = new Date();
      const currentYear = currentDate.getFullYear();

      for (let year = currentYear - 1; year <= currentYear + 1; year++) {
        for (let month = 0; month < 12; month++) {
          const daysInMonth = new Date(year, month + 1, 0).getDate();
          for (let day = 1; day <= daysInMonth; day++) {
            days.push(new Date(year, month, day));
          }
        }
      }

      return days;
    });

    const formatDate = (date) => {
      const day = date.getDate().toString().padStart(2, '0');
      const month = (date.getMonth() + 1).toString().padStart(2, '0');
      return `${day}/${month}`;
    };


// const daysInMonth = computed(() => {
//       const days = [];
//       const date = new Date();
//       const year = date.getFullYear();
//       const month = date.getMonth();
//       const daysInMonth = new Date(year, month + 1, 0).getDate();

//       for (let i = 1; i <= daysInMonth; i++) {
//         days.push(i);
//       }
    
//       return days;
//     }); 

    const getVacationClass = (vacation, day) => {
 
      const startDate = new Date(vacation.startDate);
      const endDate = new Date(vacation.endDate);

      if (startDate.getDate() <= day && endDate.getDate() >= day) {
        console.log(startDate);
        console.log(endDate);
        console.log(day);
        if (vacation.status === 'approved') return 'approved';
        if (vacation.status === 'pending') return 'pending';
        if (vacation.status === 'taken') return 'taken';
        if (vacation.status === 'cancelled') return 'cancelled';
      }
      return '';
    };

    const getVacationStyle = (vacation, day) => {
      const startDate = new Date(vacation.startDate);
      const endDate = new Date(vacation.endDate);

      if (startDate <= day && endDate >= day) {
        return {
          width: '100%'
        };
      }
      return {};
    };

</script>

<style scoped>
.gantt-container {
  display: flex;
  flex-direction: column;
  max-width: 100%;
}

.gantt-header {
  position: sticky;
  top: 0;
  background: Red;
  z-index: 2;
}

.gantt-body {
  overflow-x: auto;
}

.gantt-table {
  border-collapse: collapse;
  width: max-content;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
  white-space: nowrap;
}

.fixed-column {
  position: sticky;
  left: 0;
  background: green;
  z-index: 1;
}

.gantt-cell {
  position: relative;
  min-width: 20px;
}

.gantt-cell div {
  height: 100%;
  position: absolute;
  top: 0;
  bottom: 0;
}

.approved {
  background-color: green;
}

.pending {
  background-color: yellow;
}

.taken {
  background-color: blue;
}

.cancelled {
  background-color: red;
}
</style>



