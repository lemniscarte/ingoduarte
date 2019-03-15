<template>
  <div id="app">
    <Title msg="ingo duarte" />
  </div>
</template>

<script>
import Title from "./components/Title.vue";

export default {
  name: "app",
  components: {
    Title
  }
};
</script>

<style>
/* div {
  border: 1px solid red;
} */

@import url("https://rsms.me/inter/inter.css");

html {
  font-family: "Inter", sans-serif;
  /* margin: 0; */
  padding: 0;
}

html,
html a {
  -webkit-font-smoothing: antialiased;
  text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.004);
}

body {
  margin: 10px;
}

@supports (font-variation-settings: normal) {
  html {
    font-family: "Inter var", sans-serif;
  }
}

#app {
  margin: 0;
  padding: 0;
  /* width: calc(100vw - 20px); */
  height: calc(100vh - 20px);
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  text-align: center;
  overflow: hidden;
}
</style>
