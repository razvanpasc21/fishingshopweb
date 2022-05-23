<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <!-- <v-col md="10">
        <v-card class="center" max-width="344" outlined>
          <v-list-item three-line>
            <v-list-item-content>
              <div class="text-overline mb-4">Product name</div>
              <v-list-item-title class="text-h5 mb-1">
                Headline 5
              </v-list-item-title>
              <v-list-item-subtitle
                >Greyhound divisely hello coldly
                fonwderfully</v-list-item-subtitle
              >
            </v-list-item-content>

            <v-list-item-avatar tile size="80">
              <img
                src="https://icon-library.com/images/user-icon-free/user-icon-free-28.jpg"
              />
            </v-list-item-avatar>
          </v-list-item>

          <v-card-actions>
            <v-btn outlined rounded text center> Button </v-btn>
          </v-card-actions>
        </v-card>
      </v-col> -->
      <v-col md="10">
      <v-item-group active-class="primary">
        <v-container>
          <v-row>
            <v-col v-for="product in products" :key="product.id" md="3">
              <v-item>
                <v-card 
                    class="d-flex flex-column"
                    height="100%">
                  <v-img
                    src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
                  ></v-img>

                  <v-card-title> {{product.name}} </v-card-title>

                  <v-card-subtitle> {{product.description}} </v-card-subtitle>
                  <v-spacer class="d-flex"></v-spacer>
                  <v-card-actions>
                    <v-btn color="#fc9d03" class="mt-auto" dark @click="addToCart(product)"> Add to cart </v-btn>
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
                  <v-list-item-title>Jane Smith</v-list-item-title>
                  <v-list-item-subtitle>Logged In</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </template>

            <v-divider></v-divider>

            <v-list dense>
              <v-list-item v-for="item in items" :key="item.title">
                <v-list-item-icon>
                  <v-icon>{{ item.icon }}</v-icon>
                </v-list-item-icon>

                <v-list-item-content>
                  <v-list-item-title>{{ item.title }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
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
export default {
  name: "ShopView",

  data() {
    return {
      items: [
        { title: "Shop", icon: mdiStoreCheckOutline },
        { title: "My Account", icon: "mdi-account" },
        { title: "My Cart", icon: mdiCart },
        { title: "Orders", icon: mdiClipboardTextClock },
      ],
      products: [],
    };
  },

  methods: {
    async getProducts() {
      await axios.get("http://localhost:8090/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    async addToCart(product) {
      try {
          console.log(this.$userId)
          var body = { userId: this.$userId, productId: product.id};
          var result = await axios.post(`http://localhost:8090/cart/`, body);
          } catch(_) {
          alert("Can't add to cart! Please try again!");
        }
    },
  },

  created() {
    this.getProducts();
  },
};
</script>