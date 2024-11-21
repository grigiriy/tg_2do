<template>
  <div class="task-list mt-4 flex flex-col">
    <TaskItem
        v-for="(task, index) in tasks"
        :key="task.id"
        :task="task"
        :index="index"
        @toggle-done="toggleDone"
        @toggle-subtask="toggleSubtask"
        @delete-task="deleteTask"
      />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TaskItem from "./TaskItem.vue";

export default defineComponent({
  name: "TaskList",
  components: { TaskItem },
  props: {
    tasks: { type: Array, required: true },
  },
  emits: ["update-tasks"], // Emit updated tasks to parent
  setup(props, { emit }) {

    const toggleDone = (task) => {
      task.done = !task.done;
      emit("update-tasks", props.tasks); // Notify parent
    };

    const toggleSubtask = (task) => {
      task.isSubtask = !task.isSubtask;
      emit("update-tasks", props.tasks); // Notify parent
    };

    const deleteTask = (task) => {
      const updatedTasks = props.tasks.filter((t) => t.id !== task.id);
      emit("update-tasks", updatedTasks);
    };

    return {
      toggleDone,
      toggleSubtask,
      deleteTask,
    };
  },
});
</script>
