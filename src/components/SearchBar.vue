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
import { defineComponent } from "vue";

interface Meme {
  id: number;
  name: string;
  commands: string[];
}

export default defineComponent({
  name: "SearchBar",
  props: {
    memes: {
      type: Array,
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
      memeMap: new Map(),
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
      const memes = [
        ...new Set(
          [...this.memeMap.entries()]
            .filter(([name]: string) => {
              return name.toLowerCase().includes(this.searchText.toLowerCase());
            })
            .map((entry: [string, Meme]) => entry[1])
        ),
      ];
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
}
</style>
