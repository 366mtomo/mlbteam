<template>
  <div id="app">
    <div>
      <button v-on:click="getContents()">ボタン</button>
    </div>
    <h3>ワード</h3>
    <div v-for="(word, index) in selectWords" v-bind:key="`first-${index}`">
      {{ word }}
    </div>
    <h3>カテゴリー</h3>
    <div
      v-for="(category, index) in selectCategories"
      v-bind:key="`second-${index}`"
    >
      {{ category }}
    </div>
    <div>
      <input type="text" v-model="answerText" />
      <button v-on:click="checkAnswer()">回答</button>
    </div>
    <div>
      <a id="answer">{{ title }}</a>
    </div>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      content: "",
      words: [],
      selectWords: [],
      title: "",
      answerText: "",
      categories: [],
      selectCategories: [],
      answerURL: ""
    };
  },
  computed: {},
  methods: {
    getContents: function() {
      this.axios({
        url:
          "https://ja.wikipedia.org/w/api.php?format=json&action=query&generator=random&grnnamespace=0&prop=revisions&rvprop=content&indexpageids",
        method: "GET"
      }).then(res => {
        const pageId = res.data.query.pageids[0];
        this.content = res.data.query.pages[pageId].revisions[0]["*"];
        this.title = res.data.query.pages[pageId].title;
        this.content = res.data.query.pages[pageId].revisions[0]["*"];
        this.answerURL = "https://ja.wikipedia.org/wiki/" + this.title; //
        console.log(this.title);
        console.log(this.content);
        this.getWords(this.content);
        this.randomWordsSelect(); //
        this.randomCategoriesSelect(); //
        var target = document.getElementById("answer"); //
        target.href = this.answerURL; //
      });
    },
    getWords: function(str) {
      let array = str.split("");
      let isLink = false;
      let isDup = false;
      let word = "";
      this.words = [];
      for (let i = 1; i < array.length - 1; i++) {
        if (array[i] == "]") {
          if (array[i + 1] == "]") {
            isLink = false;
            if (word.match(/Category:/)) {
              word = word.split("Category:").join("");
              //ここから
              for (let t = 0; t < this.categories; t++) {
                if (this.categories[t] === word) {
                  isDup = true;
                }
              }
              if (!isDup) {
                this.categories.push(word);
              }
            } else {
              for (let j = 0; j < this.words; j++) {
                if (this.words[j] === word) {
                  isDup = true;
                }
              }
              if (!isDup) {
                this.words.push(word);
              }
              isDup = false;
            }
            //ここまで
            word = "";
          }
        }
        if (isLink) {
          word = word.concat(array[i]);
        }
        if (array[i] == "[") {
          if (array[i - 1] == "[") {
            isLink = true;
          }
        }
      }
      console.log(this.words);
      console.log(this.categories);
    },
    checkAnswer: function() {
      if (this.answerText == this.title) {
        console.log("正解！");
      } else {
        console.log("残念！");
      }
      this.answerText = "";
    },
    //ここから
    randomWordsSelect: function() {
      this.selectWords = this.words
        .slice()
        .sort(function() {
          return Math.random() - 0.5;
        })
        .slice(0, 9);
      console.log(this.selectWords);
    },
    randomCategoriesSelect: function() {
      this.selectCategories = this.categories
        .slice()
        .sort(function() {
          return Math.random() - 0.5;
        })
        .slice(0, 3);
      console.log(this.selectCategories);
    }
    //ここまで
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
