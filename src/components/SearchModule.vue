<template>
  <div>
    <div>
      <input v-model="search_text" />
    </div>
    <div v-if="error_message">{{ error_message }}</div>
    <ul>
      <li v-for="data in dataList" v-bind:key="data.id">{{ data.value }}</li>
    </ul>
  </div>
</template>

<script>
import debounce from "lodash/debounce";

export default {
  name: "SearchModule",
  data() {
    return {
      search_text: "",
      dataList: [],
      error_message: null
    };
  },
  mounted() {
    this.fetchResults = debounce(this.fetchResults, 400, { maxWait: 1000 });
  },
  methods: {
    fetchResults(text) {
      fetch(`https://api.chucknorris.io/jokes/search?query=${text}`)
        .then(response => response.json())
        .then(dataList => this.handleResponse(dataList));
    },
    onSearchChange(event) {
      this.search_text = event.target.value;
    },
    handleResponse(response) {
      if (response.status === 400) {
        this.error_message = response.violations["search.query"];
        this.dataList = [];
      } else {
        this.error_message = null;
        this.dataList = response.result;
      }
    }
  },
  watch: {
    search_text(newValue) {
      const MIN_CHAR_ALLOWED = 3;
      const MAX_CHAR_ALLOWED = 120;

      if (newValue.length < 3 || newValue.length > MAX_CHAR_ALLOWED) {
        this.error_message = `la taille doit être comprise entre ${MIN_CHAR_ALLOWED} et ${MAX_CHAR_ALLOWED}`;
      } else {
        this.error_message = null;
        this.fetchResults(newValue);
      }
    }
  }
};
</script>
