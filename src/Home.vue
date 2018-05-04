<template lang="html">
  <div id="home">
    <h1>{{msg}}</h1>
    <button>Googleアカウントでログイン</button>
  </div>
</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      msg: "ぷちノートへようこそ"
    }
  }
}
</script>

<style lang="css">
</style>
