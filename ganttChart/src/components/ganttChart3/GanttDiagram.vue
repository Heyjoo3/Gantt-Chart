<template>
    <div class="gantt-diagram">
        <table>
            <thead>
                <tr>
                    <th v-for="month in monthInYears" :key="month.key"  :colspan="month.days" class="top-row">{{ month.label }}</th>
                </tr>
                <tr>
                    <th v-for="day in daysInYears" :key="day.key" class="bottom-row">{{ day.label }}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in teamData" :key="employee.name">
                    <td v-for="day in daysInYears" :key="day.key" class="cell" >
                        <div v-if="isVacationDay(employee.vacations, day.date)" 
                             :class="getVacationClass(employee.vacations, day.date)">
                            &nbsp;
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>
import { computed, onMounted  } from 'vue';

const props = defineProps({
    teamData: Array,
})

const today = new Date();
const startDate =new Date( today.getFullYear()-1, 0, 1);
const endDate = new Date(today.getFullYear()+2, 0, 0);

const getDaysInYears = (start, end) => {
    const days = [];
    for (let date = new Date(start); date <= end; date.setDate(date.getDate() + 1)) {
        days.push({
            key: date.toISOString().slice(0, 10),
            label: date.getDate(),
            date: new Date(date)
        });
    }
    return days;
};

const getMonthsInYears = (start, end) => {
    const months = [];
    for (let date = new Date(start); date <= end; date.setMonth(date.getMonth() + 1)) {
        const year = date.getFullYear();
        const month = date.getMonth() + 1;
        const daysInMonth = new Date(year, month, 0).getDate();
        months.push({
            key: `${year}-${month}`,
            label: `${month}.${year}`,
            days: daysInMonth
        });
    }
    return months;
};

const daysInYears = computed(() => getDaysInYears(startDate, endDate));
const monthInYears = computed(() => getMonthsInYears(startDate, endDate));

const isVacationDay = (vacations, day) => {
    return vacations.some(vacation => {
        const vacationStartDate = new Date(vacation.startDate);
        const vacationEndDate = new Date(vacation.endDate);
        return day >= vacationStartDate && day <= vacationEndDate;
    });
};

const getVacationClass = (vacations, day) => {
    const vacation = vacations.find(vacation => {
        const vacationStartDate = new Date(vacation.startDate);
        const vacationEndDate = new Date(vacation.endDate);
        return day >= vacationStartDate && day <= vacationEndDate;
    });
    return vacation ? `vacation-${vacation.status}` : '';
};

const centerToday = () => {
    const daysPassed = Math.floor((today - startDate) / (1000 * 60 * 60 * 24)) -1;
    const diagram = document.querySelector('.gantt-diagram');
    diagram.scrollLeft = (daysPassed / daysInYears.value.length) * diagram.scrollWidth;
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
    .gantt-diagram .bottom-row:nth-child(even), 
    .gantt-diagram .top-row:nth-child(even) {
        background-color: #267ec7;
    }
    .gantt-diagram th, .gantt-diagram td {
        padding: 8px;
        text-align: left;
        min-width: 2rem;
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

