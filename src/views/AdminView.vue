<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col md="10" class="stanga">
        <h1>Admin Panel</h1>
        <h2>Choose the table you want to make changes on</h2>
        <v-card class="mx-auto" align="center" justify="space-around">
          <a href="/users">
            <v-btn dark color="#3f51b5"> Users </v-btn>
          </a>
          <a href="/products">
            <v-btn dark color="#3f51b5"> Products </v-btn>
          </a>
        </v-card>
      </v-col>
      <v-col md="2">
        <v-card height="750px">
          <v-navigation-drawer absolute permanent right>
            <template v-slot:prepend>
              <v-list-item two-line>
                <v-list-item-avatar>
                  <img
                    src="https://icon-library.com/images/user-icon-free/user-icon-free-28.jpg"
                  />
                </v-list-item-avatar>

                <v-list-item-content>
                  <v-list-item-title
                    >{{ currentUser.firstName }}
                    {{ currentUser.lastName }}</v-list-item-title
                  >
                  <v-list-item-subtitle
                    >Admin</v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item>
            </template>

            <v-divider></v-divider>

            <v-list dense>
              <a href="/shop">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>{{ items[0].icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ items[0].title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </a>
              <a href="/account">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>{{ items[1].icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ items[1].title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </a>
              <a href="/cart">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>{{ items[2].icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ items[2].title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </a>

              <a href="/orders">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>{{ items[3].icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ items[3].title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </a>

              <a href="/admin">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>{{ items[4].icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ items[4].title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </a>

              <a href="/chat">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>{{ items[5].icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ items[5].title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </a>
            </v-list>
            <v-btn class="logOutButton" dark @click="logOut">
              Log Out
            </v-btn>
          </v-navigation-drawer>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
import { mdiCart } from "@mdi/js";
import { mdiStoreCheckOutline } from "@mdi/js";
import { mdiClipboardTextClock } from "@mdi/js";
import { mdiShieldCrown } from '@mdi/js';
import { mdiFaceAgent } from '@mdi/js';

export default {
  name: "AdminView",

  data() {
    return {
      items: [
        { title: "Shop", icon: mdiStoreCheckOutline },
        { title: "My Account", icon: "mdi-account" },
        { title: "My Cart", icon: mdiCart },
        { title: "Orders", icon: mdiClipboardTextClock },
        { title: "Admin Panel", icon: mdiShieldCrown },
        { title: "Chat", icon: mdiFaceAgent },
      ],
      currentUser: null,
    };
  },

  methods: {
    async getUser() {
      await axios
        .get(`http://localhost:8090/users/${localStorage.getItem("userId")}`)
        .then((response) => {
          console.log(response.data);
          this.currentUser = response.data;
          this.userBalance = this.currentUser.balance.toFixed(2);
          if(this.currentUser.admin == 0) {
            console.log("wtf");
            alert("You do not have access here!");
            this.$router.push('/');
          }
        });
    },
    logOut() {
      localStorage.setItem("userId", -1);
      this.$router.push('/')
      console.log(result.data); 
    },
  },
  created() {
    this.getUser();
  },
};
</script> 

<style>
h1 {
  margin-top: 10px;
  margin-bottom: 50px;
}

form {
  margin-top: 50px;
  margin-left: 600px;
}

.buton {
  margin: auto;
  margin-top: 50px;
}

h2 {
  margin-bottom: 50px;
}
</style>