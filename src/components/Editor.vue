<template lang="html">
  <div id="editor">
    <h1>編集画面</h1>
    <!-- <span>{{user.displayName}}</span> -->
    <button @click = "logout">ログアウト</button>
    <div class="editorWrapper">
      <textarea class="markdown" v-model = "markdown" placeholder="テキストを入力すると、右側にプレビューが表示されます。"></textarea>
      <div class="preview" v-html = "preview()">
      </div>
    </div>
  </div>
</template>

<script>
import marked from 'marked';
export default {
  name: 'editor',
  // props: ['user'],
  data () {
    return {
      markdown: "",
     }
    },
    methods: {
      logout: function() {
        firebase.auth().signOut();
      },
      preview: function() {
        return marked(this.markdown);
      },
    }
  }
</script>

<style lang="scss" scoped>
  .editorWrapper{
    display: flex;
  }
  .markdown {
    width: 50%;
    height: 500px;
    outline: 0;
    border: 0;
    min-height: 500px;
    border: 1px solid #d6d6d6;
    border-right: 0;
    padding: 10px;
    transition: width .5s;
  }
  .preview {
    width: 50%;
    text-align: left;
    font-size: .7em;
    background-color: #fff;
    text-align: left;
    border: 1px solid #d6d6d6;
    padding: 10px;
    min-height: 500px;
    transition: width .5s;
  }
</style>
