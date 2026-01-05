<script lang="ts" setup>
import { ref } from "vue";

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

const newTask = ref("");
const error = ref("");

const formSubmitted = () => {
  if (newTask.value.trim()) {
    emit("addTask", newTask.value);
    newTask.value = "";
  } else {
    error.value = "Task title cannot be empty";
  }
};
</script>

<template>
  <main>
    <form @submit.prevent="formSubmitted">
      <label
        >New Task
        <input
          v-model="newTask"
          name="newTask"
          :aria-invalid="!!error || undefined"
          @input="error = ''"
          placeholder="Write your new task"
        />
        <small v-if="error" id="invalid-helper">{{ error }}</small>
      </label>
      <label>
        <div class="button-container">
          <button>Add</button>
        </div>
      </label>
    </form>
  </main>
</template>

<style>
#invalid-helper {
  color: red;
  text-align: left;
}
.button-container {
  display: flex;
  justify-content: right;
  margin-top: 1rem;
}
</style>
