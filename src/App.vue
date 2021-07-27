<template>
  <SearchBar v-model:matches="matches" :memes="memes" />
  {{ matches }}
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";

export default defineComponent({
  name: "App",
  components: {
    SearchBar,
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
