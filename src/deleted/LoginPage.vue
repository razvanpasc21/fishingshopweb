<template>
  <form>
    <v-row align-content="center" class="ml-5">
    <v-col lg="4" align="center">

    <v-text-field
      v-model="email"
      label="Email"
      required
    ></v-text-field>

    <v-text-field
      v-model="password"
      type="password"
      label="password"
      required
    ></v-text-field>

    <v-btn
      class="mr-4"
      color="success"
      @click="login"
    >
    Login
    </v-btn>
    <v-btn @click="clear">
      clear
    </v-btn>
    </v-col>
    </v-row>

  </form>
</template>

<script>
import axios from "axios";
export default {
  name: 'LoginPage',

  data() {
    return {
      valid: false,
      email: "",
      password: ""
    }
  }, 
  
  methods: {
    async login() {
      var body = { email: this.email, password: this.password };
      try {
        var result = await axios.post("http://localhost:8090/magazinonline/users", body);
        print(result);
      } catch(_) {
        alert("Parola gresita");
      }
    }
  }
};
</script>