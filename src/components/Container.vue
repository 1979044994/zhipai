<template>
  <div class="container">
    <div v-for="card in cards" :key="card.id" class="card" @mousedown="startDrag($event, card)" :style="{
      left: card.position.x + 'px',
      top: card.position.y + 'px',
      width: card.size.width + 'px',
      height: card.size.height + 'px',
    }">
      {{ card.name }}
    </div>

    <button
      @click="() => cards.push({ id: cards.length + 1, name: `Card ${cards.length + 1}`, position: { x: 50, y: 50 }, size: { width: 100, height: 100 } })">
      Add Card
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

interface Card {
  id: number;
  name: string;
  position: {
    x: number;
    y: number;
  };
  size: {
    width: number;
    height: number;
  };
}

const startDrag = (event: MouseEvent, card: Card) => {
  const { offsetX, offsetY } = event;

  const onMouseMove = (event: MouseEvent) => {
    card.position.x = event.clientX - offsetX;
    card.position.y = event.clientY - offsetY;
  };

  const onMouseUp = () => {
    document.removeEventListener("mousemove", onMouseMove);
    document.removeEventListener("mouseup", onMouseUp);
  };

  document.addEventListener("mousemove", onMouseMove);
  document.addEventListener("mouseup", onMouseUp);
};

const cards = ref<Array<Card>>([
  {
    id: 1,
    name: "Card 1",
    position: {
      x: 0,
      y: 0,
    },
    size: {
      width: 100,
      height: 100,
    },
  },
  {
    id: 2,
    name: "Card 2",
    position: {
      x: 10,
      y: 20,
    },
    size: {
      width: 100,
      height: 100,
    },
  },
]);
</script>

<style scoped>
.container {
  position: relative;
  width: 100%;
  height: 100%;
}

.card {
  position: absolute;
  background-color: #eee;
  border: 1px solid #ccc;
  border-radius: 5px;
  user-select: none;
  padding: 5px;
  cursor: move;
}
</style>
