<template>
  <div class="zdogtetra">
    <canvas class="zdog-canvaszdogtetra" height="150px" width="150px"></canvas>
  </div>
</template>

<script>
export default {
  name: "zdogtri",
  props: {
    color: String,
    scale: Number,
    rotationx: Number,
    rotationy: Number,
    stroke: [Number, Boolean],
    isAnimated: Boolean
  },
  data() {
    return {};
  },
  mounted() {
    let illoElem = document.querySelector(".zdog-canvaszdogtetra");
    let sceneSize = 96;
    let TAU = Zdog.TAU;
    let isSpinning = this.isAnimated;
    let viewRotation = new Zdog.Vector();
    let displaySize;

    let eggplant = "#636";
    let garnet = "#C25";
    let orange = "#E62";
    let gold = "#EA0";

    let illo = new Zdog.Illustration({
      element: illoElem,
      scale: 10,
      resize: "fullscreen",
      onResize: function(width, height) {
        displaySize = Math.min(width, height);
        this.zoom = Math.floor(displaySize / sceneSize);
      }
    });

    let solids = [];

    let tetrahedron = new Zdog.Anchor({
      addTo: illo,
      translate: { x: 0, y: 0 },
      scale: 1
    });

    let radius = 0.5;
    let inradius = Math.cos(TAU / 6) * radius;
    let height = radius + inradius;

    solids.push(tetrahedron);

    let triangle = new Zdog.Polygon({
      sides: 3,
      radius: radius,
      addTo: tetrahedron,
      translate: { y: height / 3 },
      fill: true,
      stroke: false,
      color: eggplant
      // backface: false,
    });

    for (let i = 0; i < 3; i++) {
      let rotor1 = new Zdog.Anchor({
        addTo: tetrahedron,
        rotate: { y: (TAU / 3) * -i }
      });
      let rotor2 = new Zdog.Anchor({
        addTo: rotor1,
        translate: { z: inradius, y: height / 3 },
        rotate: { x: Math.acos(1 / 3) * -1 + TAU / 4 }
      });
      triangle.copy({
        addTo: rotor2,
        translate: { y: -inradius },
        color: [gold, garnet, orange][i]
      });
    }

    triangle.rotate.set({ x: -TAU / 4, z: -TAU / 2 });

    // -- animate --- //

    let keyframes = [{ x: 0, y: 0 }, { x: 0, y: TAU }, { x: TAU, y: TAU }];

    let ticker = 0;
    let cycleCount = 180;
    let turnLimit = keyframes.length - 1;

    function animate() {
      update();
      illo.renderGraph();
      requestAnimationFrame(animate);
    }

    animate();

    function update() {
      if (isSpinning) {
        let progress = ticker / cycleCount;
        let tween = Zdog.easeInOut(progress % 1, 4);
        let turn = Math.floor(progress % turnLimit);
        let keyA = keyframes[turn];
        let keyB = keyframes[turn + 1];
        viewRotation.x = Zdog.lerp(keyA.x, keyB.x, tween);
        viewRotation.y = Zdog.lerp(keyA.y, keyB.y, tween);
        ticker++;
      }

      solids.forEach(function(solid) {
        solid.rotate.set(viewRotation);
      });

      illo.updateGraph();
    }

    // ----- inputs ----- //

    let dragStartRX, dragStartRY;

    new Zdog.Dragger({
      startElement: illoElem,
      onDragStart: function() {
        isSpinning = false;
        dragStartRX = viewRotation.x;
        dragStartRY = viewRotation.y;
      },
      onDragMove: function(pointer, moveX, moveY) {
        viewRotation.x = dragStartRX - moveY / (displaySize * TAU);
        viewRotation.y = dragStartRY - moveX / (displaySize * TAU);
      }
    });

    // let animated = this.isAnimated;

    // const illo = new Zdog.Illustration({
    //   element: ".zdog-canvaszdogtetra",
    //   scale: 20,
    //   dragRotate: true
    // });

    // let tetrahedron = new Zdog.Anchor({
    //   addTo: illo,
    //   translate: { x: 0, y: 0 },
    //   scale: 2.5
    // });

    // let radius = 0.5;
    // let inradius = Math.cos(Zdog.TAU / 6) * radius;
    // let height = radius + inradius;

    // let tetraFace = new Zdog.Polygon({
    //   sides: 3,
    //   radius: radius,
    //   addTo: tetrahedron,
    //   translate: { y: height/3 },
    //   fill: true,
    //   stroke: this.stroke,
    //   color: "rebeccapurple"
    // });

    // for (let i = 0; i < 3; i++) {
    //   let rotor1 = new Zdog.Anchor({
    //     addTo: tetrahedron,
    //     rotate: { y: Zdog.TAU/3 * -i },
    //   });
    //   let rotor2 = new Zdog.Anchor({
    //     addTo: rotor1,
    //     translate: { z: inradius, y: height/3 },
    //     rotate: { x: Math.acos(1/3) * -1 + Zdog.TAU/4 },
    //   });
    //   tetraFace.copy({
    //     addTo: rotor2,
    //     translate: { y: -inradius },
    //     color: [
    //       "tomato",
    //       "gold",
    //       "firebrick"
    //     ][i]
    //   });
    // }

    // tetraFace.rotate.set({ x: -Zdog.TAU/4, z: -Zdog.TAU/2 });

    // illo.updateRenderGraph();

    // let ticker = 0;
    // let cycleCount = 200;

    // function animate() {
    //   let progress = ticker / cycleCount;
    //   let tween = Zdog.easeInOut(progress % 1, 3);
    //   illo.rotate.y = 2 * tween * Zdog.TAU;
    //   illo.rotate.z = tween * Zdog.TAU;
    //   illo.rotate.x = tween * 0.01;
    //   if (animated) {
    //     ticker++;
    //   }
    //   illo.updateRenderGraph();
    //   requestAnimationFrame(animate);
    // }
    // animate();
  }
};
</script>

<style>
/* .zdog-canvas {
  width: 150px;
  height: 150px;
} */

.zdog {
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  text-align: center;
}

.zdog-canvaszdogtetra {
  touch-action: none;
  display: block;
  width: 100%;
  height: 60%;
  background: #cfd;
  cursor: move;
}
</style>
