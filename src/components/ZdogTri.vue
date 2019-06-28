<template>
  <div class="triangle">
    <canvas class="triangle-canvas" height="150px" width="150px"></canvas>
  </div>
</template>

<script>
// in case the npm package starts to work with the Dragger class:
// import Zdog from "zdog";
// window.Zdog = Zdog;
const ZdogObject = window.Zdog;

export default {
  name: "zdogtri",
  props: {
    color: String,
    scale: Number,
    rotationx: Number,
    rotationy: Number,
    rotationz: Number,
    stroke: Number,
    isAnimated: Boolean
  },
  data() {
    return {};
  },
  mounted() {
    let initialRotationx =
      this.rotationx == 0 ? 0 : (this.rotationx * Math.PI) / 180;
    let initialRotationy =
      this.rotationy == 0 ? 0 : (this.rotationy * Math.PI) / 180;
    let initialRotationz =
      this.rotationz == 0 ? 0 : (this.rotationz * Math.PI) / 180;

    let animated = this.isAnimated;

    const illo = new ZdogObject.Illustration({
      element: ".triangle-canvas",
      dragRotate: true
    });

    let triangle = new ZdogObject.Polygon({
      addTo: illo,
      radius: 30,
      sides: 3,
      stroke: this.stroke,
      fill: true,
      color: this.color,
      // TODO: use a red backface?
      backface: "#f00",
      scale: this.scale
    });

    let ticker = 0;
    let cycleCount = 200;

    let animate = () => {
      let progress = ticker / cycleCount;
      let tween = ZdogObject.easeInOut(progress % 1, 3);
      triangle.rotate.x = tween * 0.01 + initialRotationx;
      triangle.rotate.y = 2 * tween * ZdogObject.TAU + initialRotationy;
      triangle.rotate.z = tween * ZdogObject.TAU + initialRotationz;
      if (animated) {
        ticker++;
      }
      illo.updateRenderGraph();
      requestAnimationFrame(animate);
    };
    animate();
  }
};
</script>

<style>
.triangle-canvas {
  touch-action: none;
  /* width: 150px;
  height: 150px; */
}

.triangle {
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  text-align: center;
}
</style>
