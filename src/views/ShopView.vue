<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col md="10">
        <h1>Welcome, {{ currentUser.firstName }} {{ currentUser.lastName }} !</h1>
        <v-item-group active-class="primary">
          <v-container>
            <v-row>
              <v-col v-for="product in products" :key="product.id" md="3">
                <v-item text-center>
                  <v-card class="d-flex flex-column produs" height="100%">
                    <img v-bind:src="'' + product.image"/> 

                    <v-card-title class = "title" text-center> {{ product.name }} </v-card-title>

                    <v-card-subtitle>
                      {{ product.description }}
                    </v-card-subtitle>

                    <v-card-subtitle class="pret">
                      {{ product.price }} RON
                    </v-card-subtitle>
                    <v-spacer class="d-flex"></v-spacer>
                    <v-card-actions>
                      <v-btn
                        color="#fc9d03"
                        class="mt-auto"
                        dark
                        @click="addToCart(product)"
                      >
                        Add to cart
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-item>
              </v-col>
            </v-row>
          </v-container>
        </v-item-group>
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
                    >Balance: {{ userBalance }}$</v-list-item-subtitle
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

              <a href="/chat">
                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>{{ items[4].icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ items[4].title }}</v-list-item-title>
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
import { mdiFaceAgent } from '@mdi/js';
export default {
  name: "ShopView",

  data() {
    return {
      items: [
        { title: "Shop", icon: mdiStoreCheckOutline },
        { title: "My Account", icon: "mdi-account" },
        { title: "My Cart", icon: mdiCart },
        { title: "Orders", icon: mdiClipboardTextClock },
        { title: "Chat", icon: mdiFaceAgent },
      ],
      products: [],
      currentUser: null,
    };
  },

  methods: {
    async getProducts() {
      await axios.get("http://localhost:8090/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    async getUser() {
      await axios
        .get(`http://localhost:8090/users/${localStorage.getItem("userId")}`)
        .then((response) => {
          console.log(response.data);
          this.currentUser = response.data;
          this.userBalance = this.currentUser.balance.toFixed(2);
        });
    },
    async addToCart(product) {
      try {
        var body = {
          userId: localStorage.getItem("userId"),
          productId: product.id,
        };
        var result = await axios.post(`http://localhost:8090/cart/`, body);
        alert("Product added in cart!");
      } catch (_) {
        alert("Can't add to cart! Please try again!");
      }
    },
    logOut() {
      localStorage.setItem("userId", -1);
      this.$router.push('/')
      console.log(result.data); 
    },
  },

  created() {
    this.getProducts();
    this.getUser();
  },
};
</script>

<style scoped>
  .logOutButton {
    position: absolute;
    bottom: 0px;
    width: 130px;
    height: 130px;
    background: #0088cc;
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
  }
  h1 {
    font-size: 20px;
    margin-left: -1200px;
  }
  .pret {
    font-size: 25px;
  }
  .title {
    text-align: center;

  }
</style>