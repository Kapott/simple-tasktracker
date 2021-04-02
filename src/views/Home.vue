<template>
<AddTask v-show="showAddTask" @add-task="addTask" />
<Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
/>
<div id="notes">
    Doubleclick to toggle reminder.
</div>
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

export default {
  name: 'Home',
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
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      const res = await fetch(`api/tasks/${id}`, {
        method: 'DELETE',
      });

      if (res.status === 200) {
        this.tasks = this.tasks.filter((t) => t.id !== id);
      } else {
        alert('Error deleting task!');
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updateTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updateTask),
      });

      const data = await res.json();

      this.tasks = this.tasks
        .map((task) => (task.id === id ? { ...task, reminder: data.reminder } : task));
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },

};
</script>
