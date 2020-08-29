<template>
  <div id="app">
    <input type="text" v-model="text" />
    <button v-on:click="getContents()">ボタン</button>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      content: "",
      text: "利根川"
    };
  },
  computed: {},
  methods: {
    getContents: function() {
      this.axios({
        url: "https://ja.wikipedia.org/w/api.php",
        method: "GET",
        params: {
          format: "json",
          action: "query",
          prop: "revisions",
          rvprop: "content",
          titles: this.text
        }
      }).then(res => {
        console.log(res.data.query.pages[13901].revisions[0]["*"]);
      });

      return this.content;
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
