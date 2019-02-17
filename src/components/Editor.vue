<template>
  <div class="editor">
    <h1>
      エディタ画面
    </h1>
    <span>
      {{ user.displayName }}
    </span>
    <button @click="logout">
      ログアウト
    </button>

    <div class="editorWrapper">
      <div class="memoListWrapper">
        <!-- :data-selected (v-bind:deta-selected) と明示的に記述して内容に応じてスタイルを当てる -->
        <div class="memoList" v-for="(memo, index) in memos" @click="selectMemo(index)" :data-selected="index == selectedIndex">
          <p class="memoTitle">
            {{ displayTitle(memo.markdown) }}
          </p>
        </div>
        <button class="addMemoBtn" @click="addMemo">
          メモの追加
        </button>
        <button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo">
          選択中のメモの削除
        </button>
        <button class="saveMemoBtn" @click="saveMemos">
          メモを保存する
        </button>
      </div>
      <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
      <div class="preview markdown-body" v-html="preview()"></div>
    </div>
  </div>
</template>

<script>
import marked from "marked"

export default {
  name: "editor",
  props: ["user"],
  data() {
    // 配列にすることで複数のメモを格納できるようにする
    // Lodash などを用いて検索機能やソート機能を実装してみるのもよい
    return {
      memos: [
        {
          markdown: ""
        }
      ],
      selectedIndex: 0
    }
  },
  // コンポーネントが作成されたタイミングで実行される
  created: function() {
    firebase
      .database()
      .ref('memos/' + this.user.uid)
      .once('value') // 一回だけ、読み込みに利用することを指定している
      .then(result => {
        if (result.val()) {
          this.memos = result.val()
        }
      })
  },
  // コンポーネントの描画が完了したタイミングで実行される
  mounted: function() {
    document.onkeydown = e => {
      if (e.key == 's' && e.metaKey) {
        this.saveMemos()
        return false
      }
    }
  },
  // コンポーネントが削除されるタイミングで実行される
  beforeDestroy: function() {
    document.onkeydown = null
  },
  methods: {
    logout: function() {
      firebase.auth().signOut()
    },
    preview: function() {
      return marked(this.memos[this.selectedIndex].markdown)
    },
    addMemo: function() {
      this.memos.push({
        markdown: "無題のメモ"
      })
    },
    selectMemo: function(index) {
      this.selectedIndex = index
    },
    displayTitle: function(text) {
      return text.split(/\n/)[0]
    },
    deleteMemo: function() {
      this.memos.splice(this.selectedIndex, 1)
      if (this.selectedIndex > 0) {
        this.selectedIndex--
      }
    },
    saveMemos: function() {
      firebase
        .database()
        .ref('memos/' + this.user.uid)
        .set(this.memos)
    }
  }
}
</script>

<style lang="scss" scoped>
.editorWrapper {
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
  margin: 10px;
}

.markdown {
  width: 40%;
  height: 500px;
}

.preview {
  width: 40%;
  text-align: left;
}
</style>
