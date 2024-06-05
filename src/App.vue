<script>
import SearchComponent from "./components/SearchComponent.vue";
import ButtonComponent from "./components/ButtonComponent.vue";
import TableComponent from "./components/TableComponent.vue";
import NavigationComponent from "./components/NavigationComponent.vue";
const LIMIT = 1;

export default {
  components: {
    SearchComponent,
    ButtonComponent,
    TableComponent,
    NavigationComponent,
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
      searchFilter: "",
      inputText: "",
      inputTel: "",
      inputName: "",
      page: 1,
      firstPage: 1,
      pageLimit: 4,
      sortBy: "companyName",
      sortDir: 1,
    };
  },
  methods: {
    modalOpen() {
      this.modalShow = true;
    },
    closeModal() {
      this.modalShow = false;
    },
    addNewCompany() {
      if (this.isDisabled) {
        return;
      }
      const newCompany = {
        id: this.id++,
        companyName: this.inputText,
        directorName: this.inputName,
        phoneNumber: this.inputTel,
      };

      this.companies.push(newCompany);
      this.sort();
      this.inputText = "";
      this.inputTel = "";
      this.inputName = "";
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
    isDisabled() {
      return (
        this.inputText === "" || this.inputTel === "" || this.inputName === ""
      );
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
    <ButtonComponent @modal-opened="modalOpen" />
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
  <ModalCompnent @close="closeModal" />
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

.app__search {
  width: 200px;
  padding: 10px 20px;
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
  background-image: url("../src/assets/arrow.png");
}

.app__navigation span {
  margin: 0 10px;
}

.app__navigation .btnInv::before {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(-1);
}

.app__td-small {
  width: 40px;
}

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
