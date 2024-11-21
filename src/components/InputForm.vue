<template>
  <div class="input-form">
    <input
      type="text"
      class="border border-gray-300 rounded px-2 py-1"
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Задача"
    />
    <button
    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
    @click="addTask">Добавить</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "InputForm",
  emits: ["add-task"],
  setup(_, { emit }) {
    const newTask = ref("");

    const addTask = () => {
      if (!newTask.value.trim()) return; // Prevent adding empty tasks
      emit("add-task", newTask.value);
      newTask.value = ""; // Clear the input field
    };

    return { newTask, addTask };
  },
});
</script>

<style scoped>
.input-form {
  display: flex;
  gap: 8px;
}
</style>
