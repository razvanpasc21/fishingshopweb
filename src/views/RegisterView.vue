<template>
  <form>
    <v-row align-content="center" class="ml-5">
    <v-col lg="4" align="center">

    <v-text-field
      v-model="firstName"
      label="First Name"
      required
    ></v-text-field>

    <v-text-field
      v-model="lastName"
      label="Last Name"
      required
    ></v-text-field>

    <v-text-field
      v-model="password"
      label="Password"
      type="password"
      required
    ></v-text-field>

    <v-text-field
      v-model="email"
      label="E-mail"
      required
    ></v-text-field>

    <v-radio-group v-model="gender">
    Gender:
      <v-radio
        :key="1"
        :label="`Male`"
        :value="1"
      ></v-radio>
      <v-radio
        :key="2"
        :label="`Female`"
        :value="2"
      ></v-radio>
    </v-radio-group>

    <v-btn
      class="mr-4"
      dark
      color="#3f51b5"
      @click="register"
    >
    Register
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

    name: 'RegisterPage',

    data() {
    return {
        valid: false,
        firstName: "",
        lastName: "",
        email: "",
        password: ""
    }
    }, 

    methods: {
    async register() {
        var body = { firstName: this.firstName, lastName: this.lastName, email: this.email, password: this.password, gender: this.gender };
        try {
        var result = await axios.post("http://localhost:8090/users", body);
        print(result);
        } catch(_) {
        alert("Can't register user! Please try again!");
      }
    }
  }
};
</script> 