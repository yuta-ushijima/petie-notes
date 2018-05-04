<template>
  <div id="app">
    <div class=""  v-if = "isLogin">
      <Editor></Editor>
    </div>
    <div class=""  v-else>
      <Home></Home>
    </div>

  </div>
</template>

<script>
import Home from './components/Home.vue';
import Editor from './components/Editor.vue';

export default {
  name: 'app',
  data () {
    return {
      isLogin: false
    }
  },
  created: function() {
    firebase.auth().onAuthStateChanged(user => {
      console.log(user);
      if(user) {
        this.isLogin = true;
      } else {
        this.is_Login = false;
      };
    });
  },
  components: {
    'Home': Home,
    'Editor': Editor,
  },
}
</script>

<style lang="scss">

</style>
