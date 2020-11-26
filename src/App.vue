<template>
  <div id="app">
    <Navigation />
    <router-view class="container" :user="user" />
  </div>
</template>

<script>
// Components Import
import Navigation from "@/components/Navigation.vue";

// Firebase Imports
import Firebase from "firebase";
// eslint-disable-next-line no-unused-vars
import db from "./db.js";

export default {
  name: "app",
  data() {
    return {
      user: null
    };
  },
  mounted() {
    Firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.user = user.email;
      }
    });
  },
  components: {
    Navigation
  }
};
</script>

<style lang="scss">
$primary: brown;
@import "node_modules/bootstrap/scss/bootstrap";
</style>
