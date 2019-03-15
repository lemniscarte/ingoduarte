<template>
  <p>{{ drawTri(triSize) }}</p>
</template>

<script>
export default {
  data() {
    return {
      triSize: 100
    }
  },
  methods: {
    drawTri(side) {
      var draw = SVG('drawing')

      draw.rect(100, 100).fill('#f06')

      var polygon = draw.polygon('50,0 60,40 100,50 60,60 50,100 40,60 0,50 40,40')
      polygon.fill('#ff6')

      var side = 100;

      var h = side * (Math.sqrt(3)/2);

      var beginX = (0);
      var beginY = (-h / 2);
      var secondX = (-side / 2);
      var secondY = (h / 2);
      var endX = (side / 2);
      var endY = (h / 2);

      console.log(beginX, beginY,
                  secondX, secondY,
                  endX, endY);

      var tri = draw.polygon(`${beginX},${beginY} ${secondX},${secondY} ${endX},${endY}`);

      tri.move(0, 0);
      tri.fill('#f06');
      tri.animate("opacity: 0");
    }
  }
}
</script>

<style>

</style>
