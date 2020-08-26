<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <button v-on:click="getContents()">ボタン</button>
  </div>
</template>
<script>
export default {
  data: function() {
    return {
      content: ""
    };
  },
  computed: {},
  methods: {
    getContents: function() {
      console.log("a");
      //const display = document.getElementById("display");
      this.axios({
        url: "https://ja.wikipedia.org/w/api.php",
        method: "GET",
        params: {
          format: "json",
          action: "query",
          prop: "revisions",
          rvprop: "content",
          titles: "利根川"
        }
      }).then(res => {
        console.log(res);
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
