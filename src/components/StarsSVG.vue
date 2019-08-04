<template>
  <div>
    <svg
      class="zdog-svg"
      :width="this.windowWidth"
      :height="this.windowHeight"
    ></svg>
    <slot class="centertext"></slot>
  </div>
</template>

<script>
const ZdogObject = window.Zdog;
const TAU = ZdogObject.TAU;

const ZfontObject = window.Zfont;
Zfont.init(ZdogObject);
const font = new ZdogObject.Font({
  src: "stmr.ttf"
});

let starCount = 200;
let starRange = 1000;
let stars = [];

const zoom = 4;
const sceneWidth = 160;
const sceneHeight = 160;

let isDragActive = false;
let isSpinning = true;

export default {
  name: "starsSVG",
  data() {
    return {
      windowHeight: window.innerHeight,
      windowWidth: window.innerWidth,
      articleStars: [
        "INTRO",
        "ABOUT",
        "ANOTHER ENTRY",
        "ONE MORE ARTICLE",
        "A LONG FORM ESSAY",
        "A BRIEF DESCRIPTION OF SOMETHING"
      ]
    };
  },
  computed: {
    biggestDimension: function() {
      return this.windowHeight > this.windowWidth
        ? this.windowHeight
        : this.windowWidth;
    }
  },
  methods: {
    rand: function(min, max, ease) {
      if (max === undefined) {
        max = min;
        min = 0;
      }
      let random = Math.random();
      if (ease) {
        random = ease(Math.random(), 0, 1, 1);
      }
      return random * (max - min) + min;
    },
    map: function(val, inMin, inMax, outMin, outMax) {
      return (outMax - outMin) * ((val - inMin) / (inMax - inMin)) + outMin;
    }
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener("resize", () => {
        this.windowHeight = window.innerHeight;
        this.windowWidth = window.innerWidth;
      });
    });

    let svg = document.querySelector('svg');

    let viewWidth = sceneWidth * zoom;
    let viewHeight = sceneHeight * zoom;
    let svgWidth = svg.getAttribute('width');
    let svgHeight = svg.getAttribute('height');
    
    svg.setAttribute('viewBox', `${-viewWidth/2} ${-viewHeight/2} ` +
      `${viewWidth} ${viewHeight}`);
    
    let scene = new ZdogObject.Anchor();

    let subSceneRotating = new ZdogObject.Anchor();
    let subSceneNotRotating = new ZdogObject.Anchor();

    scene.addChild(subSceneNotRotating);
    scene.addChild(subSceneRotating);

    let starRange = this.biggestDimension / 2;

    let rectBackground = new ZdogObject.Rect({
      addTo: scene,
      width: 2000,
      height: 1000,
      stroke: false,
      color: "#000c",
      fill: true
    });

    for (let i = 0; i < starCount; i++) {
      stars.push({
        shape: new ZdogObject.Ellipse({
          addTo: subSceneRotating,
          diameter: 0,
          translate: {
            x: this.rand(-starRange, starRange),
            y: this.rand(-starRange, starRange),
            z: this.rand(-starRange, starRange)
          },
          stroke: this.rand(1.1, 2.1),
          color: `hsla(0, 0%, 100%, ${this.rand(0.3, 1)})`
        })
      });
    };

    let bigStar = new ZdogObject.Ellipse({
        // translate: {
        //   x: this.rand(-starRange / 2, starRange / 2),
        //   y: this.rand(-starRange / 2, starRange / 2),
        //   z: this.rand(-starRange / 2, starRange / 2)
        // },
        addTo: scene,
        diameter: 0,
        stroke: 20,
        color: "#f0f"
      });

    this.articleStars.forEach((article, index) => {
      let randTranslateObj = {
        x: this.rand(-starRange / 2, starRange / 2),
        y: this.rand(-starRange / 2, starRange / 2),
        z: this.rand(-starRange / 2, starRange / 2)
      };
      let starItem = new ZdogObject.Ellipse({
        translate: randTranslateObj,
        addTo: subSceneRotating,
        diameter: 0,
        stroke: 10,
        color: "#f00"
      });
  
      let starText = new ZdogObject.Text({
        translate: randTranslateObj,
        addTo: subSceneNotRotating,
        font: font,
        value: article,
        fontSize: 20,
        textAlign: 'left',
        textBaseline: 'bottom',
        color: '#fff',
        fill: true,
      });

      starText.translate.x += 10;
      starText.translate.y += -10;
      starText.rotate.x = TAU / 9;
      starText.rotate.z = -(TAU / 9);
    })

    console.log(subSceneNotRotating.children)

    let animate = () => {
      if (!isDragActive) {
        subSceneRotating.rotate.y += 0.003;
        subSceneNotRotating.rotate.y += 0.003;
        // subSceneRotating.children.forEach((text, index) => {
        //   subSceneNotRotating.children text.renderFront.y = 600;
        //   text.renderFront.x = 30;
        // });

      }

      scene.updateGraph();
      render();
      requestAnimationFrame(animate);
    };

    function render() {
      empty(svg);
      scene.renderGraphSvg(svg);
    }

    animate();

    function empty(element) {
      while (element.firstChild) {
        element.removeChild(element.firstChild);
      }
    }

    let dragStartRX, dragStartRY;
    let minSize = Math.min(svgWidth, svgHeight);

    new Zdog.Dragger({
      startElement: svg,
      onDragStart: function() {
        isDragActive = true;
        dragStartRX = subSceneRotating.rotate.x;
        dragStartRY = subSceneRotating.rotate.y;
      },
      onDragMove: function(pointer, moveX, moveY) {
        subSceneRotating.rotate.x = dragStartRX - (moveY / minSize * TAU);
        subSceneRotating.rotate.y = dragStartRY - (moveX / minSize * TAU);

        subSceneNotRotating.rotate.x = dragStartRX - (moveY / minSize * TAU);
        subSceneNotRotating.rotate.y = dragStartRY - (moveX / minSize * TAU);

        bigStar.translate.x = subSceneNotRotating.renderFront.x;
        bigStar.rotate.y = subSceneNotRotating.rotate.y / TAU;
        bigStar.rotate.z = subSceneNotRotating.rotate.z / TAU;

        subSceneNotRotating.children.forEach(title => {
          title.rotate.x = scene.rotate.x / TAU;
          title.rotate.y = scene.rotate.y / TAU;
          // title.rotate.z = scene.rotate.z / TAU;

          // title.rotate.y += rectBackground.rotate.y / TAU;
          // console.log(title.rotate.x)
          // title.translate.x = 0
        })
      },
      onDragEnd: function() {
        isDragActive = false;
      }
    });
  }
};
</script>

<style scoped>
.centertext {
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  text-align: center;
}

.zdog-svg {
  background: black;
  cursor: move;
  touch-action: none;
}
</style>
