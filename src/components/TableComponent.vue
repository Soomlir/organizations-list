<script>
export default {
  name: "TableComponent",
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
  emits: ["delete", "sort"],
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
          <img src="../assets/delete.png" width="40" height="40" />
        </a>
      </td>
    </tr>
  </table>
</template>

<style>

.app__table {
  grid-column: 1 / -1;
  min-width: 500px;
  width: 100%;
  margin-top: 40px;
  margin-right: auto;
  margin-left: auto;
  border-collapse: collapse;
}

.app__table button {
  font: inherit;
  border: none;
  background-color: transparent;
  cursor: pointer;
}

.app__table td:nth-child(3) {
  text-align: right;
}

.app__table th,
.app__table td {
  padding: 5px 10px;
  border: 1px solid gray;
}

.app__td-small {
  width: 40px;
}
</style>