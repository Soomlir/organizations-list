<script>
export default {
  props: {
    currentPage: Number,
  },
  methods: {
    decreasePage() {
      this.$emit("update-page", Math.max(this.currentPage - 1, 1));
    },
    increasePage() {
      this.$emit("update-page", this.currentPage + 1);
    },
  },
};
</script>

<template>
  <div class="app__navigation">
    <button @click="decreasePage"></button>
    <span>Страница {{ currentPage }}</span>
    <button @click="increasePage" class="btnInv"></button>
  </div>
</template>
