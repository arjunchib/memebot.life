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
    <MemeComponent
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
import MemeComponent from "./components/Meme.vue";
import memebotIcon from "./assets/icons/memebot.svg";
import { Meme } from "./models";

export default defineComponent({
  name: "App",
  components: {
    SearchBar,
    MemeComponent,
  },
  data() {
    return {
      memes: [] as Meme[],
      matches: [] as Meme[],
      searchText: "",
      memebotIcon,
    };
  },
  async created() {
    const res = await axios.get("/memes.json");
    this.memes = res.data as Meme[];
    this.matches = res.data as Meme[];
  },
  methods: {
    memeStyle(meme: Meme) {
      const isSelected = this.matches.find(
        (match: Meme) => match.id === meme.id
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
