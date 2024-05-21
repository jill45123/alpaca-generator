<template>
  <header>
    <h1>Alpaca Image Generator</h1>
  </header>
  <body>
    <div class="container">
      <div class="result">
        <div class="canvas">
          <template v-for="x in list" :key="x.category">
            <img
              :alt="x.category"
              :src="
                require(`./assets/${x.category.toLowerCase()}/${x.spec[
                  x.selected
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
        <OptionButton
          title="Accessorize your Alpaca"
          :options="list"
          :selected="selected_category"
          :onClick="onCategoryClick"
        />
        <hr />
        <OptionButton
          :title="list[selected_category].category"
          :options="list[selected_category].spec"
          :selected="selected_spec"
          :onClick="onSpecClick"
        />
      </div>
    </div>
  </body>
</template>

<script setup>
import { ref } from "vue";
import OptionButton from "./components/OptionButton.vue";
import html2canvas from "html2canvas";

var selected_category = ref(0);
var selected_spec = ref(0);
var list = ref([
  {
    category: "Hair",
    spec: ["Default", "Bang", "Curls", "Elegant", "Quiff", "Short"],
    selected: 0,
    priority: 4,
  },
  {
    category: "Ears",
    spec: ["Default", "Tilt-backward", "Tilt-forward"],
    selected: 0,
    priority: 1,
  },
  {
    category: "Accessories",
    spec: ["Earings", "Glasses", "Flower", "Headphone"],
    selected: 0,
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
    selected: 0,
    priority: 0,
  },
  {
    category: "Eyes",
    spec: ["Default", "Angry", "Naughty", "Panda", "Smart", "Star"],
    selected: 0,
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
    selected: 0,
    priority: 5,
  },
  {
    category: "Mouth",
    spec: ["Default", "Eating", "Laugh", "Tongue", "Astonished"],
    selected: 0,
    priority: 6,
  },
  {
    category: "Neck",
    spec: ["Default", "Bend-backward", "Bend-forward", "Thick"],
    selected: 0,
    priority: 2,
  },
]);

function onCategoryClick(option) {
  if (selected_category.value !== option) {
    selected_category.value = option;
    selected_spec.value = list.value[option].selected;
  }
}
function onSpecClick(option) {
  selected_spec.value = option;
  list.value[selected_category.value].selected = option;
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
  list.value.forEach((i, index) => {
    const randomValue = Math.floor(Math.random() * i.spec.length);
    i.selected = randomValue;
    if (selected_category.value === index) selected_spec.value = randomValue;
  });
}
</script>

<style lang="scss">
html {
  background-color: #f6e9e2;
}
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
    border-radius: 50px;
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
}
</style>
