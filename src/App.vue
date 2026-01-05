<script lang="ts" setup>
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import type { Task, TaskFilter } from "./types.ts";
import TaskList from "./components/TaskList.vue";
import FilterButton from "./components/FilterButton.vue";

const message = ref("Tasks App");
const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>("all");

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => total + (task.done ? 1 : 0), 0)
);

const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "todo":
      return tasks.value.filter((task) => !task.done);
    case "done":
      return tasks.value.filter((task) => task.done);
    default:
      return tasks.value;
  }
});

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}
function toggleDone(id: string) {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.done = !task.done;
  }
}
function removeTask(id: string) {
  tasks.value = tasks.value.filter((task) => task.id !== id);
}
function setFilter(newFilter: TaskFilter) {
  filter.value = newFilter;
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="tasks.length === 0">Add a task to get started</h3>
    <h3 v-else>
      {{ totalDone }} / {{ tasks.length }} tasks
      completed
    </h3>
    <div class="button-container">
      <FilterButton filter="all" @set-filter="setFilter" />
      <FilterButton filter="todo" @set-filter="setFilter" />
      <FilterButton filter="done" @set-filter="setFilter" />
    </div>
    <TaskList
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin: 1rem auto;
  text-align: left;
}
h1 {
  text-align: center;
}
</style>
