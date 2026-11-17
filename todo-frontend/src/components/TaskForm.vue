<template>
  <div id="task-form">
    <v-text-field
      v-model="text"
      label="New Task"
      placeholder="Enter a task"
      variant="outlined"
    ></v-text-field>

    <v-text-field
      v-model="date"
      label="Due Date"
      type="date"
      variant="outlined"
    ></v-text-field>

    <v-btn
      color="success"
      block
      @click="addTask"
      :disabled="!text || !date"
    >
      Add Task
    </v-btn>
  </div>
</template>

<script>
export default {
  name: 'TaskForm',

  data() {
    return {
      text: '',
      date: ''
    };
  },

  methods: {
    async addTask() {
      try {
        const response = await fetch(
          `${import.meta.env.VITE_API_URL}/api/v1/tasks/`,
          {
            method: 'POST',
            credentials: 'include',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              text: this.text,
              date: this.date
            })
          }
        );

        if (response.ok) {
          this.text = '';
          this.date = '';
          this.$emit('added');
        } else {
          console.error('Could not add task:', response.status);
        }
      } catch (error) {
        console.error('Error adding task:', error);
      }
    }
  }
};
</script>
