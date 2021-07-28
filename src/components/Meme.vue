<template>
  <h2>{{ meme.name }}</h2>
  <ul>
    <li v-for="cmd of meme.commands" :key="cmd">{{ cmd }}</li>
  </ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Meme",
  props: {
    meme: {
      type: Object,
      required: true,
    },
  },
});
</script>

<style scoped></style>
