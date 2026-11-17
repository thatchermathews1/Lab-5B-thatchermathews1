<template>
  <div id="task-list">
    <v-list>
      <v-list-item
        v-for="task in tasks"
        :key="task._id"
      >
        <template v-slot:prepend>
          <v-btn
            variant="text"
            @click="updateTask(task)"
          >
            <v-icon v-if="task.done" icon="mdi-checkbox-marked"></v-icon>
            <v-icon v-else icon="mdi-checkbox-blank-outline"></v-icon>
          </v-btn>
        </template>

        <v-list-item-title :class="{ 'text-decoration-line-through': task.done }">
          {{ task.text }}
        </v-list-item-title>

        <v-list-item-subtitle>
          {{ task.date }}
        </v-list-item-subtitle>

        <template v-slot:append>
          <v-btn
            icon="mdi-trash-can"
            color="red"
            variant="text"
            @click="deleteTask(task)"
          >
          </v-btn>
        </template>
      </v-list-item>
    </v-list>
  </div>
</template>

<script>
export default {
  name: 'TaskList',

  props: {
    tasks: {
      type: Array,
      default: () => []
    }
  },

  methods: {
    async updateTask(task) {
      const updatedTask = {
        ...task,
        done: !task.done
      };

      try {
        const response = await fetch(
          `${import.meta.env.VITE_API_URL}/api/v1/tasks/${task._id}`,
          {
            method: 'PUT',
            credentials: 'include',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(updatedTask)
          }
        );

        if (response.ok) {
          this.$emit('update');
        }
      } catch (error) {
        console.error('Error updating task:', error);
      }
    },

    async deleteTask(task) {
      try {
        const response = await fetch(
          `${import.meta.env.VITE_API_URL}/api/v1/tasks/${task._id}`,
          {
            method: 'DELETE',
            credentials: 'include'
          }
        );

        if (response.ok) {
          this.$emit('update');
        }
      } catch (error) {
        console.error('Error deleting task:', error);
      }
    }
  }
};
</script>
