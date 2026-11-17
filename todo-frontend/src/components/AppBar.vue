
<template>
  <div>
    <v-app-bar color="success" elevation="2">
      <v-app-bar-title>
        BYU CYBER 210
      </v-app-bar-title>

      <v-spacer></v-spacer>

      <v-btn
        icon="mdi-menu"
        variant="text"
        @click="drawer = !drawer"
      ></v-btn>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      temporary
      location="right"
    >
      <v-list>
        <v-list-item
          v-if="user"
          :prepend-avatar="user.photo"
          :title="user.userName"
          subtitle="Google Account"
        ></v-list-item>
      </v-list>
    </v-navigation-drawer>
  </div>
</template>

<script>
export default {
  name: 'AppBar',

  props: {
    user: {
      type: Object,
      default: null
    }
  },

  data() {
    return {
      drawer: false
    };
  }
};
</script>
