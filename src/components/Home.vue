<template>
  <div id="home">
    <h1>
      <img alt="SPA by Vue.js and Firebase" src="../assets/app_logo.png">
      <!-- {{ msg }} -->
    </h1>
    <button @click="googleLogin">
      Google アカウント でログインする
    </button>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      msg: "😁 Welcome to SPA by Vue.js and Firebase 😁"
    }
  },
  methods: {
    googleLogin: function() {
      firebase
        .auth()
        .signInWithRedirect(new firebase.auth.GoogleAuthProvider())
    }
  }
}
</script>
