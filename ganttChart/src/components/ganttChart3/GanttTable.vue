<template>
    <div class = "gantt-table">
        <table>
            <thead>
                <tr>
                    <th>Employee</th>
                    <th>Total Days</th>
                    <th>Taken Days</th>
                    <th>Planned Days</th>
                    <th>Leftover Days</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for = "employee in teamData" :key = "employee.name" class="cell">
                    <td>{{employee.name}}</td>
                    <td>{{employee.totalDays}}</td>
                    <td>{{employee.takenDays}}</td>
                    <td>{{employee.plannedDays}}</td>
                    <td>{{employee.leftoverDays}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>
import { ref } from 'vue'


const props = defineProps({
    teamData: Array,
    required: true
})  
</script>

<style scoped>
    .gantt-table {
        width: 100%;
        overflow-x: auto;
        border: solid 1px #ddd;
        border-collapse: collapse;
    }
    .gantt-table table {
        width: 100%;
        border-collapse: collapse;
    }
    .gantt-table th, .gantt-table td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
    .gantt-table th {
        background-color: #f2f2f2;
        height: 70px;
    }
    .gantt-table tr:nth-child(even) {
        background-color: #f2f2f2;
    }

    .gantt-table td {
        border-style: none solid;
        border-width: 1px;
        border-color: #ddd;
        height: 35px;
    }

</style>