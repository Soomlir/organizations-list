<script>
export default {
  props: {
    columns: {
      type: Array,
      default: [],
    },
    paginatedCompanies: {
      type: Array,
      default: [],
    },
    id: Number,
    sortBy: String,
    sortDir: Number
  },
  methods: {
    deleteItem(id) {
      this.$emit("delete", id);
    },
    sortItems(key) {
      this.$emit("sort", key);
    },
  },
  emits: ["delete"],
};
</script>

<template>
  <table class="app__table">
    <tr>
      <th v-for="{ key, title } in columns" :key="key">
        <button @click="sortItems(key)" type="button">
          {{ title }}
          <template v-if="sortBy === key">
            <template v-if="sortDir === 1">&uarr;</template>
            <template v-else>&darr;</template>
          </template>
        </button>
      </th>
      <th>Номер телефона</th>
      <th></th>
    </tr>
    <tr v-for="item in paginatedCompanies">
      <td>{{ item.companyName }}</td>
      <td>{{ item.directorName }}</td>
      <td>{{ item.phoneNumber }}</td>
      <td class="app__td-small">
        <a @click="deleteItem(item.id)" class="app__delete" href="#!">
          <img src="../src/assets/delete.png" width="40" height="40" />
        </a>
      </td>
    </tr>
  </table>
</template>
