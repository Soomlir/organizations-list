<script>
  export default {
    name: 'SearchComponent',
    props: {
      modelValue: {
        type: String,
        default: '',
      },
    },
    emits: ['update:model-value'],
    computed: {
      searchFilter: {
        get() {
          return this.modelValue;
        },
        set(value) {
          this.$emit('update:model-value', value);
        },
      },
    },
  };
</script>

<template>
  <input
      v-model="searchFilter"
      @update:model-value="page = 1"
      class="app__search"
      type="search"
      placeholder="Найти по ФИО..."
    />
</template>