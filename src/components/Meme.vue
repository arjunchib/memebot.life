<template>
  <div class="meme" @click="play">
    <h2 v-html="highlight(meme.name)"></h2>
    <p v-html="highlight(commands)"></p>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import { Meme } from "../models";

export default defineComponent({
  name: "Meme",
  props: {
    meme: {
      type: Object as PropType<Meme>,
      required: true,
    },
    searchText: {
      type: String,
      default: "",
    },
  },
  data() {
    return { audio: undefined as unknown as HTMLAudioElement };
  },
  computed: {
    commands(): string {
      return this.meme.commands.join(", ");
    },
    audioURL(): string {
      return this.meme.audio.startsWith("http")
        ? this.meme.audio
        : `http://localhost:3000${this.meme.audio}`;
    },
  },
  methods: {
    highlight(str: string): string {
      if (this.searchText.length <= 1) return str;
      return str.replace(RegExp(this.searchText, "gi"), "<b>$&</b>");
    },
    play(): void {
      if (!this.audio) {
        this.audio = new Audio(this.audioURL);
      }
      this.audio?.play();
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
