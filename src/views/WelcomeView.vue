<template>
  <v-card
    class="mx-auto"
    align="center"
    justify="space-around"
  >
  <a href="/login">
    <v-btn
      dark
      color="#085e05"
    >
      Login
    </v-btn>
  </a>
  <a href="/register">
    <v-btn
      dark
      color="#3f51b5"
    >
      Register
    </v-btn>
  </a>
  </v-card>
</template>

<script>
export default {
  name: 'AdminView',
};
</script>