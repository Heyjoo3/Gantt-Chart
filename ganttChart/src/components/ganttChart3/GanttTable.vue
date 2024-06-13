<template>
    <div class = "gantt-table">
        <table>
            <thead>
                <tr>
                    <th class="name">Teammitglied</th>
                    <th class="stats">Summe</th>
                    <th class="stats">Genom-men</th>
                    <th class="stats">Geplant</th>
                    <th class="stats">Übrig</th>
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

    .gantt-table .name {
        width: 50%;
        min-width: 120px;
    }
    .gantt-table .stats {
        width: 10%;
        writing-mode: vertical-rl;
        text-orientation: mixed;
        transform: rotate(180deg);
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
    }
</style>