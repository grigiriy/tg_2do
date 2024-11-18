<template>
  <div class="telegram-app container">
    <InputForm @add-task="addTask" />
    <TaskList :tasks="tasks" @update-tasks="updateTasks" />
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from "vue";
import InputForm from "./components/InputForm.vue";
import TaskList from "./components/TaskList.vue";

export default defineComponent({
  name: "App",
  components: { InputForm, TaskList },
  setup() {
    const tasks = reactive([]);

    const addTask = (text) => {
      tasks.push({
        id: Date.now(),
        text,
        done: false,
        isSubtask: false,
      });
    };

    const updateTasks = (updatedTasks) => {
      tasks.splice(0, tasks.length, ...updatedTasks);
    };

    return { tasks, addTask, updateTasks };
  },
});
</script>
