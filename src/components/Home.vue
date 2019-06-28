<template>
  <div class="home">
    <div class="container">
      <!-- <ZdogTri
        color="white"
        :scale="1"
        :rotationx="0"
        :rotationy="0"
        :stroke="2"
        :isAnimated="false"
      /> -->
      <Stars>
        <div class="test-in-slot">
          Use the tetrahedron to navigate
        </div>
      </Stars>
      <!-- <ZdogTetra
        color="white"
        :scale="1"
        :rotationx="0"
        :rotationy="0"
        :stroke="false"
        :isAnimated="true"
      /> -->
    </div>
  </div>
</template>

<script>
// import ZdogTri from "./ZdogTri";
// import ZdogTetra from "./ZdogTetra";
import Stars from "./Stars";

export default {
  name: "home",
  data() {
    return {};
  },
  // mounted() {
  //   var home = document.getElementsByClassName("home");
  //   home[0].classList.add("dark");
  // },
  components: {
    // ZdogTri,
    // ZdogTetra,
    Stars
  }
};
</script>

<style>
.home {
  font-family: "Share Tech Mono", monospace;
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  text-align: center;
  width: 100vw;
  height: 100vh;
  background-color: black;
  color: white;
}

.dark {
  background-color: black;
}

/* .container {
  display: grid;
} */
</style>
