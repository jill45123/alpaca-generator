<template>
  <h2>{{ title }}</h2>
  <div class="options">
    <template v-for="(x, index) in options" :key="x.category || x">
      <button @click="onClick(index)" :class="{ selected: index === selected }">
        {{ x.category || x }}
      </button>
    </template>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

defineProps({
  title: String,
  options: Object,
  selected: Number,
  onClick: Function,
});
</script>

<style scoped lang="scss">
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
.selected {
  background-color: var(--primary-color);
  color: var(--second-color);
}
</style>
