<template>
  <div class="meme" @click="play">
    <h2 v-html="highlight(meme.name)"></h2>
    <p v-html="highlight(commands)"></p>
  </div>
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
    searchText: {
      type: String,
      default: "",
    },
  },
  data() {
    return { audio: undefined };
  },
  computed: {
    commands() {
      return this.meme.commands.join(", ");
    },
    audioURL() {
      return this.meme.audio.startsWith("http")
        ? this.meme.audio
        : `http://localhost:3000${this.meme.audio}`;
    },
  },
  methods: {
    highlight(str: string) {
      return str.replace(RegExp(this.searchText, "gi"), "<b>$&</b>");
    },
    play() {
      if (!this.audio) {
        this.audio = new Audio(this.audioURL);
      }
      this.audio.play();
    },
  },
});
</script>

<style scoped>
.meme {
  border: 2px solid black;
  border-radius: 7px;
  padding: 5px;
  cursor: pointer;
}

.meme:hover {
  background: lightgray;
}

h2 {
  margin: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  font-weight: 500;
  white-space: nowrap;
}

h2:deep(b) {
  font-weight: 800;
  font-variation-settings: "opsz" 60;
}

p {
  margin: 0;
  font-weight: 400;
}

p:deep(b) {
  font-variation-settings: "opsz" 60;
}
</style>
