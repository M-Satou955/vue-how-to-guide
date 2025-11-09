<script setup lang="ts">
import { ref, onMounted } from "vue";
import HelloWorld from "./components/HelloWorld.vue";

// リアクティブな状態
const count = ref(0);

// 状態を変更し、更新をトリガーする関数。
function increment() {
  count.value++;
}

// ライフサイクルフック
onMounted(() => {
  console.log(`The initial count is ${count.value}.`);
});
</script>

<template>
  <div>
    <button @click="increment">Count is: {{ count }}</button>
    <HelloWorld msg="Hello Vue 3 + Vite + Deno!" />
  </div>
</template>

<memo lang="md"></memo>
