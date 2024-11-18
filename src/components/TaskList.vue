<template>
    <div class="task-list">
        <TaskItem v-for="(task, index) in tasks" :key="task.id" :task="task" :index="index" @toggle-done="toggleDone"
            @toggle-subtask="toggleSubtask" @drag-start="onDragStart" @drop="onDrop" />
    </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from "vue";
import TaskItem from "./TaskItem.vue";

export default defineComponent({
    name: "TaskList",
    components: { TaskItem },
    props: {
        tasks: { type: Array, required: true },
    },
    emits: ["update-tasks"],
    setup(props, { emit }) {
        const state = reactive({ dragIndex: -1 });

        const toggleDone = (task) => {
            task.done = !task.done; // Flip the 'done' status
            reorderTasks(); // Reorder the tasks to move 'done' tasks to the bottom
        };


        const toggleSubtask = (task) => {
            task.isSubtask = !task.isSubtask;
        };

        const reorderTasks = () => {
            emit(
                "update-tasks",
                [...props.tasks].sort((a, b) => Number(a.done) - Number(b.done))
            );
        };

        const onDragStart = (index) => {
            state.dragIndex = index;
        };

        const onDrop = (index) => {
            const tasks = [...props.tasks];
            const [movedTask] = tasks.splice(state.dragIndex, 1);
            tasks.splice(index, 0, movedTask);
            emit("update-tasks", tasks);
        };

        return { toggleDone, toggleSubtask, onDragStart, onDrop };
    },
});
</script>