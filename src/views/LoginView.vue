<template>
  <form class="text-md-center">
    <v-row align="center" class="text-md-center">
      <v-col cols="4">
        <v-text-field
          v-model="email"
          label="Email"
          required
          class="text-md-center"
        ></v-text-field>

        <v-text-field
          v-model="password"
          type="password"
          label="password"
          required
        ></v-text-field>

        <v-btn class="mr-4" dark color="#085e05" @click="login"> Login </v-btn>
        <v-btn @click="clear"> clear </v-btn>
      </v-col>
    </v-row>
  </form>
</template>

<script>
import axios from "axios";
import Vue from "vue";
export default {
  name: "LoginPage",

  data() {
    return {
      valid: false,
      email: "",
      password: "",
    };
  },

  methods: {
    async login() {
     console.log('sdf');

      var body = { email: this.email, password: this.password };
       await axios
        .post("http://localhost:8090/login", body)
        .then((response) => {
          if (response.data != null) {
            localStorage.setItem("userId", response.data.id);
            if (response.data.admin == 0) {
              this.$router.push("/shop");
            } else {
              this.$router.push("/admin");
            }
            console.log(response.data);
          } else {
            alert("Bad credentials! Please try again!");
          }
        });
    },
  },
};
</script>