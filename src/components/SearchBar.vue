<template>
  <input
    v-model="searchText"
    class="search-bar"
    type="search"
    placeholder="Search"
    @input="onInput"
  />
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import { Meme } from "../models";

export default defineComponent({
  name: "SearchBar",
  props: {
    memes: {
      type: Array as PropType<Meme[]>,
      required: true,
    },
    matches: {
      type: Array,
      required: true,
    },
    modelValue: {
      type: String,
      required: true,
    },
  },
  emits: ["update:matches", "update:modelValue"],
  data() {
    return {
      searchText: "",
      memeMap: new Map<string, Meme>(),
    };
  },
  watch: {
    memes() {
      this.memes.forEach((meme: Meme) => {
        return [meme.name, ...meme.commands].forEach((name) =>
          this.memeMap.set(name, meme)
        );
      });
    },
  },
  methods: {
    onInput() {
      let memes = this.memes;
      if (this.searchText.length > 1) {
        const matchedMemes = [...this.memeMap.entries()]
          .filter(([name]) => {
            return name.toLowerCase().includes(this.searchText.toLowerCase());
          })
          .map((entry: [string, Meme]) => entry[1]);
        memes = [...new Set(matchedMemes)];
      }
      this.$emit("update:matches", memes);
      this.$emit("update:modelValue", this.searchText);
    },
  },
});
</script>

<style scoped>
.search-bar {
  font-size: 32px;
  border: 2px solid black;
  border-radius: 7px;
  padding: 7px;
  font-family: Fraunces;
  appearance: none;
  -webkit-appearance: none;
  color: gray;
}
</style>
