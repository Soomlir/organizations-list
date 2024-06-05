<script>
export default {
  methods: {
    modalOpen() {
      this.$emit('modal-opened');
    }
  }
}
</script>

<template>
  <button @click="modalOpen" class="app__add" type="button">
      Добавить
    </button>
</template>