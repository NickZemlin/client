<script setup lang="ts">
import { computed, onMounted, ref, watch } from "vue";
import LeftSideLayout from "./components/layout/LeftSideLayout.vue";

const layout = ref({
  vw: Math.max(
    document.documentElement.clientWidth || 0,
    window.innerWidth || 0
  ),
  left: 0,
  get right() {
    return this.vw - this.left;
  },
});
const test = (payload: string) => {
  layout.value.left = parseInt(payload, 10);
};
const rightSideWidth = computed(() => {
  return layout.value.right + "px";
});
</script>

<template>
  <div class="app-wrap">
    <LeftSideLayout id="leftSideWrap" @left-resize="test" />
    <div class="rightSide">{{ rightSideWidth }}</div>
  </div>
</template>

<style lang="scss">
* {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app-wrap {
  width: 100vw;
  height: 100vh;
  position: fixed;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}
.rightSide {
  height: 100%;
  background-color: cornsilk;
  width: v-bind(rightSideWidth);
}
</style>
