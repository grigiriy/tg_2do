<template>
  <div class="telegram-app container min-h-screen flex justify-center items-center">
    <div class="">
      <InputForm @add-task="addTask" />
      <TaskList :tasks="tasks" @update-tasks="updateTasks" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, onMounted, ref } from "vue";
import InputForm from "./components/InputForm.vue";
import TaskList from "./components/TaskList.vue";

export default defineComponent({
  name: "App",
  components: { InputForm, TaskList },
  setup() {
    const tasks = reactive([]);
    const isInitialLoad = ref(true);

    const loadTasks = () => {
      const storedTasks = JSON.parse(localStorage.getItem("tasks") || "[]");
      tasks.splice(0, tasks.length, ...storedTasks);
    };

    const saveTasks = () => {
      if (!isInitialLoad.value) {
        localStorage.setItem("tasks", JSON.stringify(tasks));
      }
    };

    const addTask = (text) => {
      tasks.push({
        id: Date.now(),
        text,
        done: false,
        isSubtask: false,
      });
      saveTasks();
    };

    const updateTasks = (updatedTasks) => {
      tasks.splice(0, tasks.length, ...updatedTasks);
      saveTasks();
    };

    onMounted(() => {
      loadTasks();
      isInitialLoad.value = false;
    });

    return { tasks, addTask, updateTasks };
  },
});
</script>

