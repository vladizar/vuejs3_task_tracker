<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    :tasks="tasks"
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
    @discard-changes="discardChanges"
    @change-task="changeTask"
  />
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const response = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        response.status == 200
          ? (this.tasks = this.tasks.filter((task) => task.id != id))
          : alert("Error deleting the task");
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const response = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(updTask),
      });

      const data = await response.json();

      this.tasks = this.tasks.map((task) =>
        task.id == id ? { ...task, reminder: data.reminder } : task
      );
    },
    async addTask(newTask) {
      const response = await fetch("api/tasks", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(newTask),
      });

      const data = await response.json();

      this.tasks = [...this.tasks, data].sort(this.compareFunction);

      this.$emit("toggle-add-task");
    },
    async fetchAndSortTasks() {
      const response = await fetch("api/tasks");

      const data = await response.json();

      return data.sort(this.compareFunction);
    },
    async fetchTask(id) {
      const response = await fetch(`api/tasks/${id}`);

      const data = await response.json();

      return data;
    },
    discardChanges(savedTask) {
      this.tasks = this.tasks.map((task) =>
        task.id == savedTask.id ? { ...savedTask } : task
      );
    },
    async changeTask(id) {
      const task = this.tasks.find((task) => task.id == id);

      const response = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(task),
      });

      this.tasks = await this.fetchAndSortTasks();
    },
    compareFunction(task1, task2) {
      const order = {
        high: 1,
        normal: 2,
        low: 3,
      };

      return order[task1.priority] - order[task2.priority];
    },
  },
  async created() {
    this.tasks = await this.fetchAndSortTasks();
  },
  emits: ["toggle-add-task"],
};
</script>
