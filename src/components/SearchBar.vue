<template>
  <input
    v-model="searchStr"
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
  },
  emits: ["update:matches"],
  data() {
    return {
      searchStr: "",
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
              return name.toLowerCase().includes(this.searchStr.toLowerCase());
            })
            .map((entry: [string, Meme]) => entry[1])
        ),
      ];
      this.$emit("update:matches", memes);
    },
  },
});
</script>

<style scoped></style>
