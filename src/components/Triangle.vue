<template>
  <div>TRIANGLE TEST</div>
</template>

<script>
// import * as SVG from "svg.js/dist/svg";
// add this to package.json in case I want to go back to it
// "svg.js": "2.5.0"

export default {
  name: "triangle",
  data() {
    return {
      triSize: 100
    };
  }
  // mounted() {
  //   this.drawTri(this.triSize);
  // },
  // methods: {
  //   drawTri(side) {
  //     var draw = SVG("drawing");

  //     var h = side * (Math.sqrt(3) / 2);

  //     var beginX = 0;
  //     var beginY = -h / 2;
  //     var middleX = -side / 2;
  //     var middleY = h / 2;
  //     var endX = side / 2;
  //     var endY = h / 2;

  //     // console.log(beginX, beginY,
  //     // middleX, middleY,
  //     // endX, endY);

  //     var tri = draw.polygon(
  //       `${beginX},${beginY} ${middleX},${middleY} ${endX},${endY}`
  //     );

  //     tri.move(0, 0);
  //     tri.fill("#000");
  //     tri.animate("opacity: 0");
  //   }
  // }
};
</script>

<style></style>
