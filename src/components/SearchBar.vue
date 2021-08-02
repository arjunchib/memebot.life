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
import { defineComponent, PropType, ref, toRefs, watch } from "vue";
import { Meme } from "../models";

const memeMap = new Map<string, Meme>();
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
      default: "",
    },
  },
  emits: ["update:matches", "update:modelValue"],
  setup(props, { emit }) {
    const searchText = ref("");
    const { memes } = toRefs(props);

    watch(memes, () => {
      memes.value.forEach((meme: Meme) => {
        return [meme.name, ...meme.commands].forEach((name) =>
          memeMap.set(name, meme)
        );
      });
    });

    const onInput = () => {
      emit("update:modelValue", searchText.value);
      if (searchText.value.length <= 1) {
        return emit("update:matches", memes.value);
      }
      const matchedMemes = [...memeMap.entries()]
        .filter(([name]) => {
          return name.toLowerCase().includes(searchText.value.toLowerCase());
        })
        .map((entry: [string, Meme]) => entry[1]);
      emit("update:matches", [...new Set(matchedMemes)]);
    };

    return { searchText, onInput };
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
