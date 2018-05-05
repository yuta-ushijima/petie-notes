<template>
<div id="editor">
  <h1>エディター画面</h1>
  <div class="login-style">
    <span>{{ user.displayName }}</span>
    <button @click="logout">ログアウト</button>
  </div>
  <div class="editorWrapper">
    <div class="memoListWrapper">
      <div class="memoList" v-for="(memo, index) in memos" @click="selectMemo(index)" :data-selected="index == selectedIndex">
        <p class="memoTitle">{{ displayTitle(memo.markdown) }}</p>
      </div>
      <button class="addMemoBtn" @click="addMemo">メモの追加</button>
      <button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo">選択中の<br>メモの削除</button>
      <button class="saveMemosBtn" @click="saveMemos">メモの保存</button>
    </div>
    <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
    <div class="preview" v-html="preview()"></div>
  </div>
</div>
</template>

<script>
import marked from 'marked';

export default {
  name: 'editor',
  props: ['user'],
  data() {
    return {
      memos: [{
        markdown: ''
      }],
      selectedIndex: 0
    }
  },
  created: function() {
    firebase
      .database()
      .ref('memos/' + this.user.uid)
      .once('value')
      .then(result => {
        if (result.val()) {
          this.memos = result.val();
        }
      })
  },
  methods: {
    logout: function() {
      firebase.auth().signOut();
    },
    addMemo: function() {
      this.memos.push({
        markdown: '無題のメモ',
      });
    },
    deleteMemo: function() {
      this.memos.splice(this.selectedIndex, 1);
      if (this.selectedIndex > 0) {
        this.selectedIndex--;
      }
    },
    saveMemos: function() {
      firebase
        .database()
        .ref('memos/' + this.user.uid)
        .set(this.memos);
    },
    selectMemo: function(index) {
      this.selectedIndex = index;
    },
    preview: function() {
      return marked(this.memos[this.selectedIndex].markdown);
    },
    displayTitle: function(text) {
      return text.split(/\n/)[0].replace(/#\s/, '');
    },
  }
}
</script>

<style lang="scss" scoped>
.editorWrapper{
    display: flex;
}
.memoListWrapper {
    width: 20%;
    border-top: 1px solid #000;
}
.memoList {
    padding: 10px;
    box-sizing: border-box;
    text-align: left;
    border-bottom: 1px solid #000;
    &:nth-child(even) {
        background-color: #ccc;
    }
    &[data-selected="true"] {
        background-color: #ccf;
    }
}
.memoTitle {
    height: 1.5em;
    margin: 0;
    white-space: nowrap;
    overflow: hidden;
}
.addMemoBtn {
    margin-top: 20px;
}
.deleteMemoBtn {
    margin: 1px 1px;
}
.markdown {
  width: 30rem;
  height: 30rem;
  background: rgba(255, 255, 255, 0.75) -webkit-gradient( linear, left top, left bottom, from(rgba(0, 0, 0, 0)), color-stop(0.96, rgba(0, 0, 0, 0)), color-stop(0.98, rgba(0, 0, 0, 0.5)), to(rgba(0, 0, 0, 0)) );
  background-size: auto 2em;
  font-size: 1rem;
  line-height: 2rem;
  border: 1px solid #aaa;
  border-radius: 2px;
  overflow: hidden;
}
.preview {
  font-size: .7em;
  background-color: #fff;
  text-align: left;
  border: 1px solid #d6d6d6;
  padding: 10px;
  min-height: 500px;
  transition: width .5s;
}
.login-style {
  float: right;
  margin: -50px 0;
  position: relative;
}
</style>
