<template>
  <SearchBar v-model:matches="matches" :memes="memes" />
  <Meme v-for="meme of matches" :key="meme.id" :meme="meme" />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
import Meme from "./components/Meme.vue";

export default defineComponent({
  name: "App",
  components: {
    SearchBar,
    Meme,
  },
  data() {
    return {
      memes: [],
      matches: [],
    };
  },
  async created() {
    const res = await axios.get("/memes.json");
    this.memes = res.data;
  },
});
</script>

<style></style>
