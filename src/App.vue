<template>
  <header>
    <div class="title"><img class="icon" :src="memebotIcon" />memebot</div>
    <SearchBar v-model="searchText" v-model:matches="matches" :memes="memes" />
    <a
      class="btn add"
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
      :hide="!isSelected(meme)"
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
    isSelected(meme: Meme) {
      return this.matches.find((match: Meme) => match.id === meme.id);
    },
  },
});
</script>

<style scoped>
header {
  display: grid;
  align-items: center;
  margin-bottom: 10px;
  grid:
    "title search add" auto /
    auto 1fr auto;
  gap: 20px;
  position: sticky;
  top: 0;
  padding: 8px 0;
  background: linear-gradient(
    rgba(255, 255, 255, 0.99) 80%,
    rgba(255, 255, 255, 0)
  );
  margin: 0 8px 10px;
  border-radius: 0 0 7px 7px;
}

.title {
  font-size: 24px;
  font-weight: 900;
  grid-area: title;
}

.icon {
  height: 24px;
  aspect-ratio: 1;
  margin-right: 10px;
  vertical-align: middle;
}

.search-bar {
  grid-area: search;
  width: 0;
  min-width: 100%;
  justify-self: stretch;
  margin: 0;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 10px 15px -3px,
    rgba(0, 0, 0, 0.05) 0px 4px 6px -2px;
}

.add {
  grid-area: add;
  justify-self: end;
  background: white;
  box-shadow: rgba(0, 0, 0, 0.05) 0px 1px 2px 0px;
}

.memes {
  display: grid;
  grid: auto / repeat(auto-fit, minmax(250px, 1fr));
  margin: 5px;
  margin-bottom: 40px;
}

.meme {
  margin: 5px;
}

@media screen and (max-width: 800px) {
  header {
    grid:
      "title add" auto
      "search search" auto /
      auto auto;
    gap: 10px;
    top: -50px;
    border-radius: 0 0 7px 7px;
    background: rgba(255, 255, 255, 0.92);
    padding-bottom: 0;
  }
}

@media screen and (max-width: 330px) {
  .title {
    font-size: 15px;
  }
}
</style>
