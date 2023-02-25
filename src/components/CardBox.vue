<template>
  <div ref="cardBox" class="cardBox" :style="{
    width: `${position.width}px`,
    height: `${position.height}px`,
    top: `${position.top}px`,
    left: `${position.left}px`
  }"></div>
</template>

<script setup lang="ts">
import type { Position } from './HelloWorld.vue';
const props = defineProps<{ id: number, position: Position }>()


</script>

<style scoped>
.cardBox {
  background-color: rgba(50, 50, 50, 0.5);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1;
  display: flex;
  justify-content: center;
}

.cardBox:after {
  content: ' ';
  display: block;
  position: absolute;
  width: 92px;
  height: 129px;
  border: 4px solid rgba(255, 255, 255, 0.4);
  border-radius: 8px;
  box-sizing: content-box;

  /*background-image: url("@/assets/empty.png");*/
  /*background-repeat: no-repeat;*/
  /*background-position: center top;*/
}
</style>
