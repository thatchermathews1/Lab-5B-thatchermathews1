<script>
import AppBar from './components/AppBar.vue';
import TaskList from './components/TaskList.vue';
import TaskForm from './components/TaskForm.vue';

export default {
  components: {
    AppBar,
    TaskList,
    TaskForm
  },

  data() {
    return {
      tasks: [],
      update: 0,
      user: null,
      sortByDate: false,
      hideCompleted: false
    };
  },

  watch: {
    update() {
      this.getTasks();
    },

    sortByDate() {
      this.getTasks();
    },

    hideCompleted() {
      this.getTasks();
    }
  },

  async mounted() {
    await this.getUser();

    if (this.user) {
      await this.getTasks();
    }
  },

  methods: {
    async getUser() {
      try {
        const response = await fetch(
          `${import.meta.env.VITE_API_URL}/api/v1/user`,
          {
            credentials: 'include'
          }
        );

        if (response.ok) {
          this.user = await response.json();
        }
      } catch (error) {
        console.error('Error loading user:', error);
      }
    },

    async getTasks() {
      try {
        const params = new URLSearchParams({
          sortByDate: this.sortByDate,
          hideCompleted: this.hideCompleted
        });

        const response = await fetch(
          `${import.meta.env.VITE_API_URL}/api/v1/tasks/?${params}`,
          {
            credentials: 'include'
          }
        );

        if (response.ok) {
          this.tasks = await response.json();
        } else {
          console.error('Could not load tasks:', response.status);
        }
      } catch (error) {
        console.error('Error loading tasks:', error);
      }
    }
  }
};
</script>

<template>
  <v-app>
    <AppBar :user="user" />

    <v-main>
      <v-container>
        <div v-if="user">
          <h1 class="mb-4">My Tasks</h1>

          <p class="mb-4">Welcome, {{ user.userName }}!</p>

          <TaskForm @added="update++" />

          <v-row class="mt-4 mb-2">
            <v-col cols="12" sm="6">
              <v-switch
                v-model="sortByDate"
                label="Sort by date"
                color="success"
                hide-details
              ></v-switch>
            </v-col>

            <v-col cols="12" sm="6">
              <v-switch
                v-model="hideCompleted"
                label="Filter completed tasks"
                color="success"
                hide-details
              ></v-switch>
            </v-col>
          </v-row>

          <TaskList
            :tasks="tasks"
            @update="update++"
          />
        </div>

        <div v-else>
          <h1>My Tasks</h1>
          <p>Please log in to view your tasks.</p>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>
