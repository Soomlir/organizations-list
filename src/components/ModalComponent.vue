<script>
export default {
  props: ['modalShow'],
  methods: {
    closeModal() {
      this.$emit("close");
    },
  },
};
</script>

<template>
  <div class="modal" :class="{ 'modal--show': modalShow }">
    <div class="modal__content">
      <h2 class="modal__title">Добавить организацию</h2>
      <form class="modal__form">
        <input v-model="inputText" type="text" placeholder="Название" />
        <input v-model="inputTel" type="tel" placeholder="Номер телефона" />
        <input v-model="inputName" type="text" placeholder="ФИО директора" />
        <div class="modal__controls">
          <button @click="closeModal" type="button">Отмена</button>
          <button @click="addNewCompany" :disabled="isDisabled" type="button">
            ОК
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
