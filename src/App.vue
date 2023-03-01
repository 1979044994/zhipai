<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import { ref } from 'vue';
import Firework from './components/Firework.vue';
import HelloWorld from './components/HelloWorld.vue';
const show = ref(false)
const changeShow = () => {
  show.value = !show.value
}
</script>

<template>
      <div>
        <HelloWorld :changeShow="changeShow"  />
      </div>
      <div>
        <Firework v-if="show" />
      </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
