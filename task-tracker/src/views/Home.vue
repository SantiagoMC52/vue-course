<template>
    <AddTask v-show="showAddTask" @add-task="addTask"/>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

const URL = 'http://localhost:5000/tasks';

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
      const res = await fetch(URL, {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });

      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (window.confirm('Are you sure?')) {
        const res = await fetch(`${URL}/${id}`, {
          method: 'DELETE',
        });
        if (res.status === 200) {
          this.tasks = this.tasks.filter((task) => task.id !== id);
        } else {
          alert('Error deleting task'); // eslint-disable-line no-alert
        }
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updateTask = { ...taskToToggle, reminder: !taskToToggle.reminder };
      const res = await fetch(`${URL}/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updateTask),
      });

      const { reminder } = await res.json();

      this.tasks = this.tasks.map((task) => (
        task.id === id ? { ...task, reminder } : task
      ));
    },
    async fetchTasks() {
      const res = await fetch(URL);
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`${URL}/${id}`);
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
