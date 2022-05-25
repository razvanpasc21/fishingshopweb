<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col md="10" class="stanga">
        <v-app>
          <v-data-table
            :headers="headers"
            :items="cart.productList"
            class="elevation-1"
          >
            <template v-slot:top>
              <v-toolbar flat>
                <v-toolbar-title>Checkout</v-toolbar-title>
                <v-divider class="mx-4" inset vertical></v-divider>
                <v-spacer></v-spacer>
                <v-dialog v-model="dialog" max-width="500px">
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="close">
                      Cancel
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="save">
                      Save
                    </v-btn>
                  </v-card-actions>
                </v-dialog>
                <v-dialog v-model="dialogDelete" max-width="500px">
                  <v-card>
                    <v-card-title class="text-h5"
                      >Are you sure you want to delete this item?</v-card-title
                    >
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="blue darken-1" text @click="closeDelete"
                        >Cancel</v-btn
                      >
                      <v-btn
                        color="blue darken-1"
                        text
                        @click="deleteItemConfirm"
                        >OK</v-btn
                      >
                      <v-spacer></v-spacer>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </v-toolbar>
            </template>

            <template v-slot:item.actions="{ item }">
              <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
            </template>
          </v-data-table>
          <v-btn
            class="d-flex justify-center align-center buton primary"
            width="150px"
            height="50px"
            @click="placeOrder"
          >
            Place order
          </v-btn>
        </v-app>
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
                  <v-list-item-subtitle>Balance: {{userBalance}}$</v-list-item-subtitle>
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
  name: "CartView",

  data() {
    return {
      items: [
        { title: "Shop", icon: mdiStoreCheckOutline },
        { title: "My Account", icon: "mdi-account" },
        { title: "My Cart", icon: mdiCart },
        { title: "Orders", icon: mdiClipboardTextClock },
        { title: "Chat", icon: mdiFaceAgent },
      ],
      headers: [
        { text: "Name", value: "name" },
        { text: "Description", value: "description" },
        { text: "Price", value: "price" },
        { text: "Quantity", value: "quantity" },
        { text: "", value: "actions" },
      ],
      dialogDelete: false,
      cart: null,
      currentUser: null,
      selectedProductIndex: -1,
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
        });
    },
    async getCart() {
      try {
        await axios
          .get(`http://localhost:8090/cart/`, {
            params: {
              userId: localStorage.getItem("userId"),
            },
          })
          .then((response) => {
            console.log(response.data);
            this.cart = response.data;
          });
      } catch (_) {
        alert("Can't get the cart! Please try again!");
      }
    },
    async deleteProduct(id) {
      try {
        var result = await axios.delete(`http://localhost:8090/cart/${id}`);
      } catch (_) {
        alert("Can't delete product! Please try again!");
      }
    },
    async placeOrder() {
      var id = localStorage.getItem("userId");
      try {
        await axios
          .post(`http://localhost:8090/orders/${id}`)
          .then((response) => {
            console.log(response.data);
            if(response.data == true) {
              this.getCart();
              alert("The order has been succesfully placed!");
            }
            else {
              alert("Can't place the order! Please try again!");
            }
          }).catch(error => {
            console.log(error.response);
          });
      } catch (_) {
        alert("Can't place the order! Please try again!");
      }
    },
    deleteItem(item) {
      this.selectedProductIndex = this.cart.productList.indexOf(item);
      console.log("Lezgo:" + this.selectedProductIndex);
      this.dialogDelete = true;
      console.log(item);
    },

    deleteItemConfirm() {
      this.deleteProduct(
        this.cart.productList[this.selectedProductIndex].cartId
      );
      this.cart.productList.splice(this.selectedProductIndex, 1);
      this.closeDelete();
    },
    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.selectedProductIndex = -1;
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
    this.getCart();
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
</style>