<template>
    <div class="gantt-diagram">
        <table>
            <thead>
                <tr>
                    <th v-for="month in monthInYears" :key="month"  :colspan="month[2]" class="top-row">{{ month[1] }}.{{month[0]}}</th>
                </tr>
                <tr>
                    <th v-for="day in daysInYears" :key="day" class="bottom-row">{{ day[2] }}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in teamData" :key="employee.name">
                    <td v-for="day in daysInYears" :key="day" class="cell" >
                        <div v-if="isVacationDay(employee.vacations, day)" 
                             :class="getVacationClass(employee.vacations, day)">
                            &nbsp;
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>
import { computed } from 'vue';
import { onMounted } from 'vue';

const props = defineProps({
    teamData: Array,
    required: true
})


const today = new Date();
const startDate =new Date( today.getFullYear()-1, 0, 2);
const endDate = new Date(today.getFullYear()+2, 0, 0);
let daysTotal = 0;

const daysInYears = computed(() => {
    const days = [];
    for (let date = new Date(startDate); date <= endDate; date.setDate(date.getDate() + 1)) {
        let splitDate = date.toISOString().slice(0, 10).split('-');
        days.push(splitDate);
    }
    return days;
});

const monthInYears = computed(() => {
    const months = [];
    for (let date = new Date(startDate); date <= endDate; date.setMonth(date.getMonth() + 1)) {
        let splitDate = date.toISOString().slice(0, 7).split('-');
        let year = splitDate[0];
        let month = splitDate[1];
        const daysInMonth = (year, month) => new Date(year, month, 0).getDate();
        splitDate.push(daysInMonth(year, month));
        months.push(splitDate);
    }
    return months;
});

// const getVacationClass = (vacation, day) => {
//     const vacationStartDate = new Date(vacation.startDate);
//     const vacationEndDate = new Date(vacation.endDate);
//     const dayDate = new Date(day[0], day[1] - 1, day[2]);
//     if (dayDate >= vacationStartDate && dayDate <= vacationEndDate) {
//         return `vacation-${vacation.status}`;
//     }
//     return '';
// };

const isVacationDay = (vacations, day) => {
    const dayDate = new Date(day[0], day[1] - 1, day[2]);
    return vacations.some(vacation => {
        const vacationStartDate = new Date(vacation.startDate);
        const vacationEndDate = new Date(vacation.endDate);
        return dayDate >= vacationStartDate && dayDate <= vacationEndDate;
    });
};

const getVacationClass = (vacations, day) => {
    const dayDate = new Date(day[0], day[1] - 1, day[2]);
    const vacation = vacations.find(vacation => {
        const vacationStartDate = new Date(vacation.startDate);
        const vacationEndDate = new Date(vacation.endDate);
        return dayDate >= vacationStartDate && dayDate <= vacationEndDate;
    });
    return vacation ? `vacation-${vacation.status}` : '';
};

const centerToday = () => {
    daysTotal = daysInYears.value.length ;
    const daysPassed = Math.floor((today - startDate) / (1000 * 60 * 60 * 24));
    console.log(daysPassed);
    var diagram = document.querySelector('.gantt-diagram');
    diagram.scrollLeft = (daysPassed / daysTotal) * diagram.scrollWidth;
};

const goLeft = () => {
    var diagram = document.querySelector('.gantt-diagram');
    diagram.scrollTo({
        left: diagram.scrollLeft - diagram.clientWidth / 2,
        behavior: 'smooth'
    });
};

const goRight = () => {
    var diagram = document.querySelector('.gantt-diagram');
    diagram.scrollTo({
        left: diagram.scrollLeft + diagram.clientWidth / 2,
        behavior: 'smooth'
    });
};


onMounted(() => {
    centerToday();
});

defineExpose({
  centerToday,
  goLeft,
  goRight,
});
</script>

<style scoped>
   .gantt-diagram {
        width: 100%;
        overflow-x: scroll;
        border-radius:  0 1rem 0 1rem ;
    }
    .gantt-diagram table {
        width: 100%;
        border-collapse: collapse;
    }
    .gantt-diagram thead {
        position: sticky;
        height: 70px;
        box-shadow: #1a2935 0px 2px 5px;
        z-index: 2;
    }
/*     .gantt-diagram .bottom-row , .gantt-diagram .top-row:nth-child(even) {
        border-style: solid solid none ;
        border-width: 1px;
        border-color: #ddd;
        box-sizing: border-box;
        
    } */
    .gantt-diagram .bottom-row:nth-child(even), .gantt-diagram .top-row:nth-child(even) {
        background-color: #267ec7;
    }
    .gantt-diagram th, .gantt-diagram td {
        padding: 8px;
        text-align: left;
    }
    .gantt-diagram th {
        text-align: center;
        background-color: #09599c;
        color: white;
    }
    .gantt-diagram td {
        position: relative;
    }
    .gantt-diagram tr:nth-child(even) {
        background-color: #f2f2f2;
    }
    .gantt-diagram tr:nth-child(odd) {
        background-color: white;
    }
    .gantt-diagram .vacation-approved {
        background-color: #4CAF50;
    }
    .gantt-diagram .vacation-pending {
        background-color: #FFEB3B;
    }
    .gantt-diagram .vacation-taken {
        background-color: #2196F3;
    }
    .gantt-diagram .vacation-cancelled {
        background-color: #f44336;
    }
    .gantt-diagram .cell {
        height: 35px;
        z-index: 1;
    }
    .gantt-diagram .cell div {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
    }
</style>

