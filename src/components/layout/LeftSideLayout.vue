<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from "vue";

const emit = defineEmits(["leftResize"]);

const makeResizableDiv = (div: HTMLElement, resizer: HTMLElement) => {
  const element = div;
  const resizers = [resizer];
  let original_width = 0;
  const minWidth = 50;
  const maxWidth = 500;
  let original_x = element.getBoundingClientRect().left;
  let original_mouse_x = 0;
  resizer.addEventListener("mousedown", function (e) {
    e.preventDefault();
    original_width = parseFloat(
      getComputedStyle(element, null)
        .getPropertyValue("width")
        .replace("px", "")
    );
    original_x = element.getBoundingClientRect().left;
    original_mouse_x = e.pageX;
    window.addEventListener("mousemove", resize);
    window.addEventListener("mouseup", stopResize);
  });

  function resize(e: MouseEvent) {
    const width = original_width + (e.pageX - original_mouse_x);
    if (width < minWidth || width > maxWidth) return;
    element.style.width = width + "px";
    localStorage.setItem("leftPanelWidth", width + "px");
    emit("leftResize", width);
  }

  function stopResize() {
    window.removeEventListener("mousemove", resize);
  }
};

const getLocalWidth = (el: HTMLElement) => {
  const width = localStorage.getItem("leftPanelWidth");
  if (typeof width === "string") {
    el.style.width = localStorage.getItem("leftPanelWidth")!;
  }
};

const leftSideWrap = ref<HTMLElement | null>(null);
const resizer = ref<HTMLElement | null>(null);
onMounted(() => {
  if (leftSideWrap.value && resizer.value) {
    makeResizableDiv(leftSideWrap.value, resizer.value);
    getLocalWidth(leftSideWrap.value);
  }
});
</script>

<template>
  <div class="leftSide-wrap" ref="leftSideWrap">
    <div class="leftSide-resizeBar" ref="resizer" />
    <h3>PFM animations</h3>
  </div>
</template>

<style scoped lang="scss">
.leftSide {
  &-wrap {
    height: 100%;
    width: 300px;
    min-width: 50px;
    max-width: 500px;
    background-color: cornflowerblue;
    position: relative;
    overflow: hidden;
    * {
      white-space: nowrap;
    }
  }
  &-resizeBar {
    box-sizing: content-box;
    padding: 0px 5px;

    position: absolute;
    right: 0px;
    height: 100%;
    // width: 5px;
    // background-color: coral;
    border-right: 1px solid black;
    cursor: col-resize;
    display: flex;
    flex-direction: column;
  }
}
</style>
