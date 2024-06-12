<template>
    <div class="gantt-diagram">
        <table>
            <thead>
                <tr>
                    <th v-for="month in monthInYears" :key="month"  :colspan="month[2]">{{ month[1] }}.{{month[0]}}</th>
                </tr>
                <tr>
                    <th v-for="day in daysInYears" :key="day">{{ day[2] }}</th>
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
import { computed, defineProps } from 'vue';
import { onMounted } from 'vue';

const props = defineProps({
    teamData: Array,
    required: true
})

const today = new Date();
const startDate =new Date( today.getFullYear()-1, 0, 2);
const endDate = new Date(today.getFullYear()+2, 0, 0);

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
    const daysTotal = daysInYears.value.length +24;
    const daysPassed = Math.floor((today - startDate) / (1000 * 60 * 60 * 24));
    console.log(daysPassed);
    var diagram = document.querySelector('.gantt-diagram');
    diagram.scrollLeft = (daysPassed / daysTotal) * diagram.scrollWidth;
};

onMounted(() => {
    centerToday();
});
</script>

<style scoped>
   .gantt-diagram {
        width: 100%;
        overflow-x: scroll;
        border: solid 1px #ddd;
    }
    .gantt-diagram table {
        width: 100%;
        border-collapse: collapse;
    }
    .gantt-diagram th, .gantt-diagram td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
    .gantt-diagram th {
        background-color: #f2f2f2;
        text-align: center;
    }
    .gantt-diagram td {
        position: relative;

    }
    .gantt-diagram tr:nth-child(even) {
        background-color: #f2f2f2;
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
        border:none;
        height: 35px;
    }

    .gantt-diagram .cell div {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
    }

</style>

