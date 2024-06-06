<script>
import SearchComponent from "./components/SearchComponent.vue";
import ButtonComponent from "./components/ButtonComponent.vue";
import TableComponent from "./components/TableComponent.vue";
import NavigationComponent from "./components/NavigationComponent.vue";
import ModalComponent from "./components/ModalComponent.vue";
const LIMIT = 1;

export default {
  components: {
    SearchComponent,
    ButtonComponent,
    TableComponent,
    NavigationComponent,
    ModalComponent,
  },
  data() {
    return {
      modalShow: false,
      columns: [
        {
          key: "companyName",
          title: "Название",
        },
        {
          key: "directorName",
          title: "ФИО директора",
        },
      ],
      id: 3,
      companies: [
        {
          id: 1,
          companyName: 'ООО "Вектор"',
          directorName: "Иванов И.И.",
          phoneNumber: "+7 000 123 45 67",
        },
        {
          id: 2,
          companyName: "ИП Сидоров С.С.",
          directorName: "Сидоров С.С.",
          phoneNumber: "+7 000 56 78 99",
        },
      ],
      inputName: "",
      page: 1,
      firstPage: 1,
      pageLimit: 4,
      sortBy: "companyName",
      sortDir: 1,
      searchFilter: ""
    };
  },
  methods: {
    modalOpen() {
      this.modalShow = true;
    },
    closeModal() {
      this.modalShow = false;
    },
    addNewCompany(dataItem) {
      const newCompany = {
        id: this.id++,
        companyName: dataItem.inputText,
        directorName: dataItem.inputName,
        phoneNumber: dataItem.inputTel,
      };
      this.companies.push(newCompany);
      this.sort();
      this.closeModal();
    },
    deleteCompany(id) {
      this.companies = this.companies.filter((elem) => elem.id !== id);
    },
    sort() {
      this.page = 1;
      this.companies.sort(
        (a, b) => a[this.sortBy].localeCompare(b[this.sortBy]) * this.sortDir
      );
    },
    sortItems(key) {
      if (this.sortBy !== key) {
        this.sortDir = 1;
      } else {
        this.sortDir = 0 - this.sortDir;
      }
      this.sortBy = key;
      this.sort();
    },
    updatePage(newPage) {
      const pageNumber = Math.min(Math.max(1, newPage), this.lastPage);
      this.page = pageNumber;
    },
  },
  computed: {
    filteredCompanies() {
      return this.companies.filter(({ directorName }) => {
        return this.searchFilter
          ? directorName.toLowerCase().includes(this.searchFilter.toLowerCase())
          : true;
      });
    },
    paginatedCompanies() {
      const startIndex = (this.page - 1) * LIMIT;
      return this.filteredCompanies.slice(startIndex, startIndex + LIMIT);
    },
    lastPage() {
      return Math.ceil(this.filteredCompanies.length / LIMIT);
    },
  },
  created() {
    this.sort();
  },
};
</script>

<template>
  <section class="app">
    <h1 class="app__title">Справочник организаций</h1>
    <SearchComponent v-model="searchFilter" />
    <ButtonComponent class="app__add" @click="modalShow = true"
      >Добавить</ButtonComponent
    >
    <TableComponent
      :columns
      :paginatedCompanies
      @delete="deleteCompany"
      @sort="sortItems"
      :sortBy
      :sortDir
    />
    <NavigationComponent :currentPage="page" @update-page="updatePage" />
  </section>
  <ModalComponent @close="closeModal" @create="addNewCompany" :modalShow />
</template>

<style scoped>
.app {
  display: grid;
  grid-template-columns: 1fr 1fr;
  width: 750px;
  margin-right: auto;
  margin-left: auto;
}

.app__title {
  grid-column: 1 / -1;
  font-size: 30px;
  text-align: center;
  color: rgba(0, 0, 0, 0.7);
}

.app__add {
  justify-self: end;
  width: 200px;
  font-size: 18px;
  padding: 6px 10px 7px;
  color: gray;
  background-color: transparent;
  border: 1px solid #000000;
  cursor: pointer;
}

.modal--show {
  display: flex;
}
</style>
