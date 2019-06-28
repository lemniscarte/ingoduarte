<template>
  <div>
    <canvas width="2000px" height="1000px"></canvas>
    <!-- <svg width="2000px" height="1000px"></svg> -->
    <!-- <div
      class="test"
      :top="windowHeight / 2 + 'px'"
      :left="windowWidth / 2 + 'px'"
    >
      Pick a star
    </div> -->
    <slot class="centertext"></slot>
  </div>
</template>

<script>
const ZdogObject = window.Zdog;

export default {
  name: "stars",
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
      ],
      illo: "",
      starAnchor: "",
      stars: [],
      starCount: 200,
      starRange: 1000,
      canvas: document.querySelector("canvas"),
      svg: document.querySelector("svg")
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

    let ZfontObject = window.Zfont;

    Zfont.init(ZdogObject);
    const font = new ZdogObject.Font({
      src: "stmr.ttf"
    });

    const canvas = document.querySelector("canvas");
    const svg = document.querySelector("svg");
    const test = document.querySelectorAll(".test");

    let illo;
    let starAnchor;
    let rectBackground;
    let stars = [];
    let starCount = 200;
    let starRange = 1000;
    let articleStars = [
      "INTRO",
      "ABOUT",
      "ANOTHER ENTRY",
      "ONE MORE ARTICLE",
      "A LONG FORM ESSAY",
      "A BRIEF DESCRIPTION OF SOMETHING"
    ];

    rectBackground = new ZdogObject.Rect({
      width: 2000,
      height: 1000,
      stroke: false,
      color: "#0009",
      fill: true
    });

    starAnchor = new ZdogObject.Anchor();

    illo = new ZdogObject.Illustration({
      element: canvas,
      dragRotate: starAnchor
    });

    illo.addChild(rectBackground);
    illo.addChild(starAnchor);

    for (let i = 0; i < starCount; i++) {
      stars.push({
        shape: new ZdogObject.Ellipse({
          addTo: starAnchor,
          diameter: 0,
          translate: {
            x: this.rand(-starRange, starRange),
            y: this.rand(-starRange, starRange),
            z: this.rand(-starRange, starRange)
          },
          stroke: this.rand(1.1, 2.1),
          color: `hsla(0, 0%, 100%, ${this.rand(0.3, 1)})`
          // color: "#ff0",
        })
      });
    }

    let locationCoords = [];

    for (let i = 0; i <= articleStars.length; i++) {
      let locationIndex = parseInt(this.rand(0, stars.length));
      locationCoords[locationIndex] = stars[locationIndex].shape.translate;
    }

    // locationCoords = rand()

    // articleStars.forEach(article => {

    // })

    let starItemsAnchor = new ZdogObject.Anchor();

    let textsAnchor = new ZdogObject.Anchor();

    this.articleStars.forEach((article, index) => {
      let randTranslateObj = {
        x: this.rand(-this.biggestDimension/2, this.biggestDimension / 2),
        y: this.rand(-this.biggestDimension/2, this.biggestDimension / 2),
        z: this.rand(-this.biggestDimension/2, this.biggestDimension / 2)
      };
      let starItem = new ZdogObject.Ellipse({
        addTo: starItemsAnchor,
        diameter: 0,
        stroke: 10,
        translate: randTranslateObj,
        // stroke: this.rand(1.1, 2.1),
        // color: `hsla(0, 0%, 100%, ${rand(0.3, 1)})`
        color: "#f00"
      });
  
      let starText = new ZdogObject.Text({
        translate: randTranslateObj,
        addTo: textsAnchor,
        font: font,
        value: article,
        fontSize: 20,
        textAlign: 'left',
        textBaseline: 'bottom',
        color: '#fff',
        fill: true,
      });

      starText.translate.x += 30;
      starText.translate.y += -10;
    })
    
    starAnchor.addChild(textsAnchor);
    starAnchor.addChild(starItemsAnchor);

    // console.log(stars);

    let childrenArray = textsAnchor.children.length;

    let animate = () => {
      // starAnchor.rotate = {
      //   x: starAnchor.rotate.y += 0.01,
      //   y: this.y += 0.005,
      //   y: this.z += 0.001
      // }

      starAnchor.rotate.y += 0.005;
      starAnchor.rotate.x -= 0.001;
      starAnchor.rotate.z -= 0.0001;

      for (let i = 0; i < childrenArray; i++) {
        // textsAnchor.children[i].rotate = {
        //   // x: starAnchor.rotate.x,
        //   y: -starAnchor.rotate.y,
        //   // z: -textsAnchor.rotate.z
        // };

        // textsAnchor.children[i].rotate.x = -starAnchor.rotate.x;
        textsAnchor.children[i].rotate.y = -starAnchor.rotate.y; // one of these works, but not with others around it!!!
        // textsAnchor.children[i].rotate.z = rectBackground.rotate.z;

        // if (textsAnchor.children[i] translate)
      }

      // textsAnchor.rotate.x = rectBackground.rotate.x * -1;
      // textsAnchor.rotate.y = rectBackground.rotate.y * -1;
      // textsAnchor.rotate.z = rectBackground.rotate.z * -1;

      // starText.rotate.y = this.map(starAnchor.rotate.y, -1, 1, 1, this.windowHeight);
      // starText.rotate.x = starItem.rotate.x * -1;
      // starText.rotate.z = starAnchor.rotate.z;

      // starText.translate.y = this.map(starAnchor.rotate.y, -1, 1, 1, this.windowHeight);
      // starText.translate.x = starAnchor.rotate.x;
      // starText.translate.z = starAnchor.rotate.z;

      // console.log(test[0]);

      // test[0].style.transform = `
      //   translate3d(${starAnchor.rotate.x * window.innerWidth}px,
      //   ${starAnchor.rotate.y * window.innerHeight}px,
      //   ${starAnchor.rotate.z * window.innerHeight}px)
      // `;

      // for (let i = 0, len = stars.length; i < len; i++) {
      //   let star = stars[i].shape;
      //   star.stroke += 0.01;
      //   // star.stroke = this.map(
      //   //   Math.sin(Date.now() * 0.002 + i * 0.4),
      //   //   -1,
      //   //   1,
      //   //   1,
      //   //   10
      //   // );
      // }

      illo.updateRenderGraph();
      window.requestAnimationFrame(animate);
    };

    animate();
  }
};
</script>

<style scoped>
canvas {
  background-color: black;
  cursor: grab;
  display: block;
  touch-action: none;
}

canvas:active {
  cursor: grabbing;
}

.centertext {
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  text-align: center;
}

/* .test {
  position: absolute;
  top: 300px;
  left: 500px;
} */
</style>
