<template>
  <AddTask v-show="shouldShowAddTask" @add-task="addTask" />

  <Tasks
    :tasks="tasks"
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
  />
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
  name: "Home",
  components: {
    Tasks,
    AddTask,
  },
  props: {
    shouldShowAddTask: Boolean,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async fetchTasks() {
      const response = await fetch("api/tasks");
      const data = await response.json();
      return data;
    },
    async fetchTask(id) {
      const response = await fetch(`api/tasks/${id}`);
      const data = await response.json();
      return data;
    },
    async addTask(task) {
      const response = await fetch("api/tasks", {
        method: "post",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await response.json();

      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        if (!res.status === 200) {
          alert("Error deleting task");
        }

        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
      const data = await res.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
