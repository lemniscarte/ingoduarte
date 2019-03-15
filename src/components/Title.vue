<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h4>making things with code</h4>
    <p
      class="floating"
      @click="isZoomed = !isZoomed"
      :class="{ big: isZoomed }"
    >
      ▼
    </p>
  </div>
</template>

<script>
export default {
  name: "Title",
  data() {
    return {
      isZoomed: false
    };
  },
  props: {
    msg: String
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
.floating {
  transform: translatey(0px);
  animation: float 3s ease-in-out infinite;
  text-shadow: 0px 5px 2px rgba(0, 0, 0, 0.6);
  cursor: pointer;
  display: inline-block;
  transition: 2.5s;
}

.floating::after {
  text-shadow: 3px 15px 15px rgba(0, 0, 0, 0.2);
  opacity: 0;
  transition: opacity 0.3s ease-in-out;
}

.floating:hover::after {
  opacity: 1;
}

.big {
  font-size: 6000px;
}

@keyframes float {
  50% {
    text-shadow: 3px 15px 15px rgba(0, 0, 0, 0.2);
    transform: translatey(-10px);
  }
}
</style>
