<script>
export default {
  name: "NavigationComponent",
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

<style>

.app__navigation {
  grid-column: 1 / -1;
  display: flex;
  align-items: center;
  width: 140px;
  margin-top: 40px;
  margin-left: auto;
}

.app__navigation button {
  position: relative;
  width: 20px;
  height: 20px;
  padding: 0;
  background: transparent;
  border: none;
  cursor: pointer;
}

.app__navigation button::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 30px;
  height: 30px;
  background-size: cover;
  background-image: url("/img/assets/arrow.png");
}

.app__navigation span {
  margin: 0 10px;
}

.app__navigation .btnInv::before {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(-1);
}
</style>