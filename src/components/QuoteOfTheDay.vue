<template>
  <h2 v-if="loading">Loading ...</h2>
  <div v-else>
    <p>{{ quote.value }}</p>
    <button type="button" v-on:click="fetchRandomQuote">Another one</button>
  </div>
</template>

<script>
const TIMEOUT = 300;

export default {
  name: "QuoteOfTheDay",

  data() {
    return {
      loading: true,
      quote: {},
      last_save: null
    };
  },

  mounted() {
    this.fetchRandomQuote();
  },

  methods: {
    showLogin() {
      let current_quote_last_save = Number(this.last_save);
      let is_not_initial_fetch = current_quote_last_save !== 0;

      setTimeout(() => {
        let request_hasnt_finished_yet =
          current_quote_last_save === this.last_save;

        if (is_not_initial_fetch && request_hasnt_finished_yet) {
          this.loading = true;
        }
      }, TIMEOUT);
    },
    hideLogin() {
      this.loading = false;
    },
    fetchRandomQuote() {
      this.showLogin();
      fetch("https://api.chucknorris.io/jokes/random")
        .then(response => response.json())
        .then(quote => this.saveQuote(quote));
    },
    saveQuote(quote) {
      this.quote = quote;
      this.last_save = Date.now();
      this.hideLogin();
    }
  }
};
</script>
