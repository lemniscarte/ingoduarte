<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h4>making things with code</h4>
    <p class="floating">▼</p>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
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
  opacity: 0.9;
  cursor: pointer;
  display: inline-block;
}
@keyframes float {
  0% {
    text-shadow: 0px 5px 2px rgba(0, 0, 0, 0.6);
    transform: translatey(0px);
  }
  50% {
    text-shadow: 3px 15px 15px rgba(0, 0, 0, 0.2);
    transform: translatey(-10px);
  }
  100% {
    text-shadow: 0px 5px 2px rgba(0, 0, 0, 0.6);
    transform: translatey(0px);
  }
}
</style>
