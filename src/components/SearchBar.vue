<template>
  <input
    :value="modelValue"
    class="search-bar"
    type="search"
    placeholder="Search"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "SearchBar",
  props: {
    modelValue: {
      type: String,
      default: "",
    },
  },
  emits: ["update:modelValue"],
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
