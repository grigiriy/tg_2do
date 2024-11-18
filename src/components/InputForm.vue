<template>
  <div class="input-container">
    <input type="text" v-model="taskText" placeholder="Enter a task" class="input" />
    <button @click="addTask" class="button">Add</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "InputForm",
  emits: ["add-task"],
  setup(_, { emit }) {
    const taskText = ref("");

    const addTask = () => {
      const text = taskText.value.trim();
      if (text) {
        emit("add-task", text);
        taskText.value = "";
      }
    };

    return { taskText, addTask };
  },
});
</script>