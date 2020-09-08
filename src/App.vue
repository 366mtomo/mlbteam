<template>
  <div id="app">
    <div>
      <button v-on:click="getContents()">ボタン</button>
    </div>
    <div>
      <input type="text" v-model="answerText" />
      <button v-on:click="checkAnswer()">回答</button>
    </div>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      content: "",
      words: [],
      title: "",
      answerText: "",
      categories: []
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
        console.log(this.title);
        console.log(this.content);
        this.getWords(this.content);
      });
    },
    getWords: function(str) {
      let array = str.split("");
      let isLink = false;
      let word = "";
      this.words = [];
      for (let i = 1; i < array.length - 1; i++) {
        if (array[i] == "]") {
          if (array[i + 1] == "]") {
            isLink = false;
            if (word.match(/Category:/)) {
              word = word.split("Category:").join("");
              this.categories.push(word);
            } else {
              this.words.push(word);
            }
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
    }
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
