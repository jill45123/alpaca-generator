<template>
  <header>
    <h1>Alpaca Image Generator</h1>
  </header>
  <body>
    <div class="container">
      <div class="result">
        <div class="canvas">
          <template v-for="x in dict" :key="x.category">
            <img
              :alt="x.category"
              :src="
                require(`./assets/${x.category.toLowerCase()}/${x.spec[
                  x.value
                ].toLowerCase()}.png`)
              "
              :style="{ zIndex: x.priority }"
            />
          </template>
          <img alt="Nose" src="./assets/nose.png" style="z-index: 3" />
        </div>
        <div class="action">
          <button @click="onRandom()">Random</button>
          <button @click="onDownload()">Download</button>
        </div>
      </div>
      <div class="accessorize">
        <div class="categories">
          <h2>Accessorize your Alpaca</h2>
          <div class="options">
            <template v-for="(x, index) in dict" :key="x.category">
              <button
                @click="onCategoryClick(index)"
                :class="{ select: index === selectedCategory }"
              >
                {{ x.category }}
              </button>
            </template>
          </div>
          <hr />
        </div>
        <div class="specs">
          <h2>{{ dict[selectedCategory].category }}</h2>
          <div class="options">
            <template
              v-for="(x, index) in dict[selectedCategory].spec"
              :key="x"
            >
              <button
                @click="onSpecClick(index)"
                :class="{ select: index === selectedSpec }"
              >
                {{ x }}
              </button>
            </template>
          </div>
        </div>
      </div>
    </div>
  </body>
</template>

<script setup>
import { ref } from "vue";
import html2canvas from "html2canvas";

var selectedCategory = ref(0);
var selectedSpec = ref(0);
var dict = ref([
  {
    category: "Hair",
    spec: ["Default", "Bang", "Curls", "Elegant", "Quiff", "Short"],
    value: 0,
    priority: 4,
  },
  {
    category: "Ears",
    spec: ["Default", "Tilt-backward", "Tilt-forward"],
    value: 0,
    priority: 1,
  },
  {
    category: "Accessories",
    spec: ["Earings", "Glasses", "Flower", "Headphone"],
    value: 0,
    priority: 6,
  },
  {
    category: "Backgrounds",
    spec: [
      "Blue50",
      "Blue60",
      "Blue70",
      "Darkblue30",
      "Darkblue50",
      "Darkblue70",
      "Green50",
      "Green60",
      "Green70",
      "Grey40",
      "Grey70",
      "Grey80",
      "Red50",
      "Red60",
      "Red70",
      "Yellow50",
      "Yellow60",
      "Yellow70",
    ],
    value: 0,
    priority: 0,
  },
  {
    category: "Eyes",
    spec: ["Default", "Angry", "Naughty", "Panda", "Smart", "Star"],
    value: 0,
    priority: 5,
  },
  {
    category: "Leg",
    spec: [
      "Default",
      "Bubble-tea",
      "Cookie",
      "Game-console",
      "Tilt-backward",
      "Tilt-forward",
    ],
    value: 0,
    priority: 5,
  },
  {
    category: "Mouth",
    spec: ["Default", "Eating", "Laugh", "Tongue", "Astonished"],
    value: 0,
    priority: 6,
  },
  {
    category: "Neck",
    spec: ["Default", "Bend-backward", "Bend-forward", "Thick"],
    value: 0,
    priority: 2,
  },
]);

function onCategoryClick(value) {
  if (this.selectedCategory !== value) {
    this.selectedCategory = value;
    this.selectedSpec = this.dict[this.selectedCategory].value;
  }
}
function onSpecClick(value) {
  this.selectedSpec = value;
  this.dict[this.selectedCategory].value = value;
}
function onDownload() {
  // seek the element(.canvas) and read it as Canvas
  html2canvas(document.querySelector(".canvas")).then(function (canvas) {
    // create a <a> tag
    var a = document.createElement("a");
    // convert the Canvas content to base64
    a.href = canvas
      .toDataURL("image/png")
      .replace("image/png", "image/octet-stream");
    a.download = "alpaca.png";
    a.click();
  });
}
function onRandom() {
  this.dict.forEach((i, index) => {
    const randomValue = Math.floor(Math.random() * i.spec.length);
    i.value = randomValue;
    if (this.selectedCategory === index) this.selectedSpec = randomValue;
  });
}
</script>

<style lang="scss">
#app {
  font-family: "Alfa Slab One", cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  overflow-x: hidden;
  width: 100%;
  height: 100%;
}
header {
  text-align: center;
  h1 {
    font-size: 3rem;
  }
}
.container {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 50px;
  margin-top: 100px;
}
.result {
  height: 800px;
  width: 700px;
  .canvas {
    position: relative;
    height: 700px;
    width: 700px;
    img {
      position: absolute;
      height: 700px;
      width: 700px;
      object-fit: contain;
    }
  }
}
.action {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-top: 20px;
  gap: 20px;
  button {
    flex: 1;
    --primary-color: #fff;
    --second-color: #783434;
    box-sizing: border-box;
    border: 1px solid;
    border-radius: 5px;
    color: var(--primary-color);
    background-color: var(--second-color);
    padding: 1em 2em;
    display: flex;
    transition: 0.2s;
    align-items: center;
    font-weight: bold;
    line-height: 50px;
  }
  button:hover {
    background-color: var(--primary-color);
    color: var(--second-color);
    cursor: pointer;
  }
}
.accessorize {
  height: 700px;
  width: 700px;
  .options {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
    margin: 30px 0px;
  }
  button {
    --primary-color: #783434;
    --second-color: #fff;
    box-sizing: border-box;
    border: 1px solid;
    border-radius: 50px;
    color: var(--primary-color);
    background-color: var(--second-color);
    padding: 1em 1.8em;
    display: flex;
    transition: 0.2s;
    align-items: center;
    gap: 0.6em;
    font-weight: bold;
  }
  button:hover {
    background-color: var(--primary-color);
    color: var(--second-color);
    cursor: pointer;
  }
  .select {
    background-color: var(--primary-color);
    color: var(--second-color);
  }
}
</style>
