<template>
  <v-card
    class="mx-auto"
    align="center"
    justify="space-around"
  >
  <a href="/users">
    <v-btn
      dark
      color="#3f51b5"
    >
      Users
    </v-btn>
  </a>
  <a href="/products">
    <v-btn
      dark
      color="#3f51b5"
    >
      Products
    </v-btn>
  </a>
  </v-card>
</template>

<script>
export default {
  name: 'AdminView',
};
</script>