<template>
  <div id="home">
    <h1>
      {{ msg }}
    </h1>
    <button>
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
  }
}
</script>

