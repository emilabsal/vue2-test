<template>
    <draggable
        class="task-grid"
        id="task-grid"
        v-model="tasks"
        group="people"
        animation=250
        fallback-tolerance=3
        @start="drag = true"
        @end="drag = false"
    >
        <TaskGridItem
            v-for="task in tasks"
            :key="task.id"
            :task="task"
            @task-delete="$emit('task-delete', task.id)"
            @task-toggle="$emit('task-toggle', task.id)"
        />
        <span
            class="no-tasks-message"
            v-if="tasks.length === 0"
        >No task for today :)</span>
    </draggable>
</template>

<script>
import TaskGridItem from "@/components/TaskGridItem.vue"
import draggable from 'vuedraggable'

export default {
    components: {
        draggable,
        TaskGridItem,
    },
    computed: {
        tasks: {
            get: function () {
                return this.$store.state.tasks
            },
            set: function (value) {
                this.$store.commit('updateTasks', value)
            }
        },
    },
}
</script>

<style scoped>
.task-grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

@media (min-width: 576px) {
    .task-grid {
        margin-top: 20px;
        margin: 40px 0 0 0;
    }
}

.no-tasks-message {
    font-size: 26px;
    color: #FFF2;
    margin-top: 100px;
}

@media (min-width: 576px) {
    .no-tasks-message {
        font-size: 32px;
    }
}
</style>