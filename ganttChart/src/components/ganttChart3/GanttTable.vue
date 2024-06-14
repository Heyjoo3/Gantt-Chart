<template>
    <div class = "gantt-table">
        <table>
            <thead>
                <tr>
                    <th class="name-header">Teammitglied</th>
                    <th class="stats-header">Summe</th>
                    <th class="stats-header">Genom-men</th>
                    <th class="stats-header">Geplant</th>
                    <th class="stats-header">Übrig</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for = "employee in teamData" :key = "employee.name" class="cell">
                    <td class="name">{{employee.name}}</td>
                    <td class="stats">{{employee.totalDays}}</td>
                    <td class="stats">{{employee.takenDays}}</td>
                    <td class="stats">{{employee.plannedDays}}</td>
                    <td class="stats">{{employee.leftoverDays}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>

const props = defineProps({
    teamData: Array,
    required: true
})  
</script>

<style scoped>
    .gantt-table {
        width: 100%;
        overflow-x: scroll;
        border-collapse: collapse;
        border-radius: 1rem  0  0 0;
        border-style: none solid none none;
        border-width: 1px;
    }
    .gantt-table table {
        width: 100%;
        border-collapse: collapse;
    }
    .gantt-table th, .gantt-table td {
        padding: 8px;
        text-align: left;
    }
    .gantt-table td {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
    .gantt-table .name
    .gantt-table .name-header {
        width: 100%;
        min-width: 120px;
        max-width: 350px;
    }
    .gantt-table .stats-header{
        width: 3rem;
        min-width: 2rem;
        max-width: 4rem;
        writing-mode: vertical-rl;
        text-orientation: mixed;
        transform: rotate(180deg);
    }
    .gantt-table .stats {
        text-align: center;
    }
    .gantt-table thead {
        position: sticky;
        box-shadow: #1a2935 2px 2px 5px;
        z-index: 2;
    }
    .gantt-table th {
        background-color: #09599c;
        color: white;
        height: 70px;
    }
    .gantt-table tr:nth-child(even) {
        background-color: #f2f2f2;
    }
    .gantt-table tr:nth-child(odd) {
        z-index: 1;
        background-color: white;
    }
    .gantt-table td {
        height: 35px;
        max-width: 300px;
    }
</style>