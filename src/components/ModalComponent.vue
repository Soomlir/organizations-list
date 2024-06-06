<script>
export default {
  name: "ModalComponent",
  data() {
    return {
      searchFilter: "",
      inputText: "",
      inputTel: "",
      inputName: "",
    };
  },
  props: {
    modalShow: {
      type: Boolean,
    },
    modelValue: {
      type: String,
      default: "",
    },
  },
  methods: {
    closeModal() {
      this.inputText = "";
      this.inputTel = "";
      this.inputName = "";
      this.$emit("close");
    },
    create() {
      this.$emit('create', {
        inputText: this.inputText,
        inputTel: this.inputTel,
        inputName: this.inputName
      });
      this.inputText = "";
      this.inputTel = "";
      this.inputName = "";
    },
    isDisabled() {
      return (
        this.inputText === "" || this.inputTel === "" || this.inputName === ""
      );
    },
  },
  emits: ["close", "update:model-value", "create"],
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
          <button @click="create" :disabled="isDisabled()" type="button">
            ОК
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<style>
.modal {
  display: none;
}

.modal--show {
  display: flex;
}

.modal__content {
  width: 420px;
  margin: auto;
  color: #ffffff;
  background-color: gray;
}

.modal__title {
  text-align: center;
}

.modal__form {
  display: flex;
  flex-direction: column;
}

.modal__form input {
  width: 250px;
  margin-right: auto;
  margin-left: auto;
  margin-bottom: 10px;
  padding: 5px 10px;
}

.modal__controls {
  display: flex;
  justify-content: space-between;
  width: 275px;
  margin-top: 20px;
  margin-right: auto;
  margin-bottom: 20px;
  margin-left: auto;
}

.modal__controls button {
  padding: 5px 10px;
}
</style>
