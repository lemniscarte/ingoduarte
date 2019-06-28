<template>
  <div id="app">
    <transition name="dissolve" mode="out-in">
      <component
        :is="this.selectedComponent"
        @pageWasChanged="changeSelectedComponent($event)"
      ></component>
    </transition>
  </div>
</template>

<script>
import Welcome from "./components/Welcome.vue";
import Home from "./components/Home.vue";

export default {
  name: "app",
  components: {
    Welcome,
    Home
  },
  data() {
    return {
      selectedComponent: "Welcome"
      // receivedEvent: ""
    };
  },
  methods: {
    changeSelectedComponent() {
      this.selectedComponent = "Home";
      // this.receivedEvent = ev;
      // console.log(this.receivedEvent)
    }
  },
  mounted() {}
};
</script>

<style>
@import url("https://rsms.me/inter/inter.css");

html {
  font-family: "Inter", sans-serif;
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
  height: calc(100vh - 20px);
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  text-align: center;
  overflow: hidden;
}

.dissolve-enter-active,
.dissolve-leave-active {
  transition: opacity 0.5s ease;
}

.dissolve-enter,
.dissolve-leave-to {
  opacity: 0;
}
</style>
