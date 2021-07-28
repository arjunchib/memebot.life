<template>
  <SearchBar v-model="searchText" v-model:matches="matches" :memes="memes" />
  <div class="memes">
    <Meme
      v-for="meme of memes"
      :key="meme.id"
      :meme="meme"
      :search-text="searchText"
      :style="memeStyle(meme)"
    />
  </div>
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
      searchText: "",
    };
  },
  async created() {
    const res = await axios.get("/memes.json");
    this.memes = res.data;
  },
  methods: {
    memeStyle(meme: any) {
      const isSelected = this.matches.find(
        (match: any) => match.id === meme.id
      );
      return {
        order: isSelected ? "" : "1",
        visibility: isSelected ? "" : "hidden",
      };
    },
  },
});
</script>

<style scoped>
.search-bar {
  margin: 20px auto;
  display: block;
}

.memes {
  display: grid;
  grid: auto / repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}
</style>
