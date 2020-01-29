<template>
  <div id="app">
    <HomePage msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import HomePage from "./components/Homepage.vue";

const CATEGORY_LIST_STORAGE_KEY = "@chuck-norris/category-list";
const CATEGORY_LIST_LAST_FETCHED_STORAGE_KEY = "@chuck-norris/last-fetched";

export default {
  name: "app",
  components: { HomePage },
  data() {
    return {
      fetching_category_list: false,
      category_list: []
    };
  },
  mounted() {
    this.fetchCategoryList();
  },
  methods: {
    saveCategoryList(category_list) {
      this.fetching_category_list = false;
      localStorage.setItem(
        CATEGORY_LIST_LAST_FETCHED_STORAGE_KEY,
        new Date().toLocaleDateString()
      );
      localStorage.setItem(
        CATEGORY_LIST_STORAGE_KEY,
        JSON.stringify(category_list)
      );
      this.category_list = category_list;
    },
    shouldWeFetchCategoryListForToday() {
      let last_fetched_date = localStorage.getItem(
        CATEGORY_LIST_LAST_FETCHED_STORAGE_KEY
      );
      let today = new Date().toLocaleDateString();

      console.log({
        last_fetched_date,
        today
      });
      return last_fetched_date !== today;
    },
    fetchCategoryList() {
      if (this.shouldWeFetchCategoryListForToday()) {
        this.fetching_category_list = true;

        fetch("https://api.chucknorris.io/jokes/categories")
          .then(response => response.json())
          .then(category_list => this.saveCategoryList(category_list));
      }
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
