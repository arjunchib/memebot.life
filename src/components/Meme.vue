<template>
  <div class="meme" :class="{ hidden: hide }" @click="play">
    <!-- eslint-disable vue/no-v-html -->
    <h2 v-html="highlight(meme.name)"></h2>
    <p v-html="highlight(commands)"></p>
    <!-- eslint-enable vue/no-v-html -->
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, PropType } from "vue";
import { Meme } from "../models";

let audio: HTMLAudioElement | undefined;

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
    hide: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const audioURL = computed((): string => {
      return props.meme.audio.startsWith("http")
        ? props.meme.audio
        : `http://localhost:3000${props.meme.audio}`;
    });
    const commands = computed((): string => {
      return props.meme.commands.join(", ");
    });
    const highlight = (str: string): string => {
      if (props.searchText.length <= 1) return str;
      return str.replace(RegExp(props.searchText, "gi"), "<b>$&</b>");
    };
    const play = (): void => {
      if (!audio) audio = new Audio(audioURL.value);
      audio?.play();
    };
    return { audioURL, commands, highlight, play };
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

.hidden {
  order: 1;
  visibility: hidden;
  height: 0;
  padding: 0;
  border: 0;
  margin: 0 !important;
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
