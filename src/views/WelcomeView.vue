<template>
  <v-card align="center" text-center>
    <h1>Welcome to the Fisherman's Hub!</h1>
    <a href="/login">
      <v-btn dark color="#085e05"> Login </v-btn>
    </a>
    <a href="/register">
      <v-btn dark color="#3f51b5"> Register </v-btn>
    </a>

  </v-card>
</template>

<script>
export default {
  name: "AdminView",
};
</script>

<style scoped>
h1 {
  margin-top: 50px;
  padding-top: 50px;
}
button {
  margin: 50px 20px 100px 20px;
  size: 100px;
}

</style>  

