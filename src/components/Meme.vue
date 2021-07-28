<template>
  <div class="meme">
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
  computed: {
    commands() {
      return this.meme.commands.join(", ");
    },
  },
  methods: {
    highlight(str: string) {
      return str.replace(RegExp(this.searchText, "gi"), "<b>$&</b>");
    },
  },
});
</script>

<style scoped>
.meme {
  border: 2px solid black;
  border-radius: 7px;
  padding: 5px;
}

h2 {
  margin: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  font-weight: 400;
}

p {
  margin: 0;
}
</style>
