<template>
  <div class="hello">
    <h1>ingo duarte</h1>
    <h4>making things with code</h4>
    <div class="arrow" @click.once="enterHome($event)">
      <ZdogTri
        color="black"
        :scale="1"
        :rotationx="0"
        :rotationy="0"
        :rotationz="180"
        :stroke="4"
        :isAnimated="true"
      />
    </div>
  </div>
</template>

<script>
import ZdogTri from "./ZdogTri";

export default {
  name: "welcome",
  components: {
    ZdogTri
  },
  data() {
    return {};
  },
  methods: {
    enterHome(ev) {
      this.$emit("pageWasChanged", ev);
    }
  }
};
</script>

<style>
h1 {
  font-size: calc(30px + 6vw);
  margin: 0;
  padding: 0;
  opacity: 0.9;
  font-weight: 100;
}

h4 {
  font-family: serif;
  font-variant: all-small-caps;
  font-size: calc(10px + 1vw);
  margin-top: calc(2px + 1vw);
  margin-bottom: calc(2px + 1vw);
  font-weight: 100;
  opacity: 0.5;
}

.arrow {
  cursor: pointer;
}
</style>
