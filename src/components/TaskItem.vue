<template>
    <div class="task-item" :class="{ done: task.done, subtusk: task.isSubtask }" draggable="true" @dragstart="startDrag"
        @dragover.prevent @drop="onDrop">
        <label>
            <input type="checkbox" :checked="task.done" @input="toggleDone" />
            <span>{{ task.text }}</span>
        </label>
        <button @click="$emit('toggle-subtask', task)" class="tab-button">→|</button>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
    name: "TaskItem",
    props: {
        task: { type: Object, required: true },
        index: { type: Number, required: true },
    },
    emits: ["toggle-done", "toggle-subtask", "reorder-tasks"],
    setup(props, { emit }) {
        const toggleDone = () => {
            emit("toggle-done", props.task);
        };

        const startDrag = () => {
            emit("drag-start", props.index);
        };

        const onDrop = () => {
            emit("drop", props.index);
        };

        return { toggleDone, startDrag, onDrop };
    },
});
</script>