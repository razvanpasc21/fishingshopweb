<template>
  <v-row>
    <v-col md="10">
      <div>
        <div id="main-content" class="container">
          <div class="row">
            <div class="col-md-6">
              <form class="form-inline">
                <div class="form-group">
                  <label for="name">Type a message below</label>
                  <input
                    type="text"
                    id="name"
                    class="form-control mesaj"
                    v-model="send_message"
                    placeholder="Write your message here..."
                    outlined
                  />
                </div>
                <v-btn
                  id="send"
                  blue
                  dark
                  class="btn btn-default"
                  type="submit"
                  @click.prevent="send"
                  >Send</v-btn
                >
              </form>
            </div>
          </div>
          <div class="row">
            <div class="col-md-12">
              <table id="conversation" class="table table-striped">
                <thead>
                  <tr>
                    <th>Chat Box</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in received_messages" :key="item">
                    <td>
                      {{ item }}
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
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
</template>

<script>
import * as SockJS from "sockjs-client";
import Stomp from "webstomp-client";
import axios from "axios";
import { mdiCart, mdiDotsGrid, mdiSelectGroup } from "@mdi/js";
import { mdiStoreCheckOutline } from "@mdi/js";
import { mdiClipboardTextClock } from "@mdi/js";
import { mdiFaceAgent } from '@mdi/js';

export default {
  name: "SocketView",
  data: function () {
    return {
        items: [
        { title: "Shop", icon: mdiStoreCheckOutline },
        { title: "My Account", icon: "mdi-account" },
        { title: "My Cart", icon: mdiCart },
        { title: "Orders", icon: mdiClipboardTextClock },
        { title: "Chat", icon: mdiFaceAgent },
      ],
      received_messages: [],
      send_message: null,
      connected: false,
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
        });
    },
    send() {
      console.log("Send message:" + this.send_message);
      if (this.stompClient && this.stompClient.connected) {
        const msg = { message: this.send_message, userFullName: this.currentUser.firstName + ' ' + this.currentUser.lastName };
        this.stompClient.send("/app/hello", JSON.stringify(msg), {});
      }
    },
    connect() {
      this.socket = new SockJS("http://localhost:8090/fishingShop-WebSocket");
      this.stompClient = Stomp.over(this.socket);
      this.stompClient.connect(
        {},
        (frame) => {
          this.connected = true;
          console.log(frame);
          this.stompClient.subscribe("/topic/greetings", (msg) => {
            console.log(msg);
            this.received_messages.push(JSON.parse(msg.body).content);
          });
        },
        (error) => {
          console.log(error);
          this.connected = false;
        }
      );
    },
    logOut() {
      localStorage.setItem("userId", -1);
      this.$router.push('/')
      console.log(result.data); 
    },
  },
  mounted() {
    this.connect();
  },
  created() {
    this.getUser();
  },
};
</script>

<style>
.mesaj {
    height: 50px;
    width: 200px;
}
</style>
