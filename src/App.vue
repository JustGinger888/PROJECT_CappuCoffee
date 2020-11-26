<template>
  <div id="app">
    <Navigation :user="user" @logout="logout" />
    <router-view
      class="container"
      :user="user"
      :groups="groups"
      @addGroup="addGroup"
      @deletegroup="deletegroup"
    />
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
      user: null,
      groups: []
    };
  },
  methods: {
    logout() {
      Firebase.auth()
        .signOut()
        .then(() => {
          this.user = null;
          this.$router.push("login");
        })
        .catch(err => {
          console.error(err);
        });
    },
    addGroup(payload) {
      db.collection("users")
        .doc(this.user.uid)
        .collection("groups")
        .add({
          name: payload,
          createAt: Firebase.firestore.FieldValue.serverTimestamp()
        });

      db.collection("groups").add({
        name: payload,
        createAt: Firebase.firestore.FieldValue.serverTimestamp()
      });
    },
    deletegroup(payload) {
      db.collection("users")
        .doc(this.user.uid)
        .collection("groups")
        .doc(payload)
        .delete();
    }
  },
  mounted() {
    Firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.user = user;

        db.collection("users")
          .doc(this.user.uid)
          .collection("groups")
          .orderBy("name")
          .onSnapshot(snapshot => {
            const snapData = [];
            snapshot.forEach(doc => {
              snapData.push({
                id: doc.id,
                name: doc.data().name
              });
            });
            this.groups = snapData;
          });
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
