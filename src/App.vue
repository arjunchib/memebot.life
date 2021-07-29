<template>
  <header>
    <img class="icon" :src="memebotIcon" /><span class="title">memebot</span>
    <SearchBar v-model="searchText" v-model:matches="matches" :memes="memes" />
    <a
      class="btn"
      href="https://discord.com/api/oauth2/authorize?client_id=350733228098715658&permissions=2048&scope=bot"
    >
      Add to server
    </a>
  </header>
  <main class="memes">
    <Meme
      v-for="meme of memes"
      :key="meme.id"
      :meme="meme"
      :search-text="searchText"
      :style="memeStyle(meme)"
    />
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
import Meme from "./components/Meme.vue";
import memebotIcon from "./assets/icons/memebot.svg";

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
      memebotIcon,
    };
  },
  async created() {
    const res = await axios.get("/memes.json");
    this.memes = res.data;
    this.matches = res.data;
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
header {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.icon {
  height: 24px;
}

.title {
  font-size: 24px;
  font-weight: 900;
  margin-left: 10px;
}

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
