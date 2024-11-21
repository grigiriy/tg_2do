<template>
  <div
  class="task-item"
  :class="{ done: task.done, subtusk: task.isSubtask }"
  >
    <label>
      <input
      type="checkbox"
      :checked="task.done"
      v-model="task.done"
      @input="toggleDone"
      />
      <span class="mx-1">{{ task.text }}</span>
    </label>
    <button @click="$emit('toggle-subtask', task)" class="tab-button cursor-pointer mr-2">→|</button>
    <button @click="$emit('delete-task', task)" class="delete-button cursor-pointer">❌</button>
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
  emits: ["toggle-done", "toggle-subtask", "delete-task"],
  setup(props, { emit }) {

    const toggleDone = () => {
      emit("toggle-done", props.task);
    };

    return {
      toggleDone
    };
  },
});
</script>

<style scoped>
.task-item {
  transition: transform 0.2s ease;
  order: 1;
}

.task-item.subtusk {
  padding-left: 30px;
}
.task-item.subtusk .tab-button {
  transform: rotate(180deg);
}
.task-item.done {
  opacity: .5;
  text-decoration: line-through;
  order: 5;
}
</style>
