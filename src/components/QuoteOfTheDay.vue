<template>
  <div>
    <div class="loading" v-if="loading">Loading ...</div>
    <div v-else>
      <p v-html="quote.value" />
    </div>
    <button
      :disabled="loading"
      v-bind:class="{ 'disabled-styles': loading }"
      class="fetch-random-quote-button"
      type="button"
      v-on:click="fetchRandomQuote"
    >
      Another one
    </button>
  </div>
</template>

<style>
.chuck-norris-name {
  font-weight: bolder;
}
</style>
<style scoped>
button.disabled-styles {
  background-color: #63c787;
}
.loading {
  font-size: 30px;
  font-weight: bolder;
  color: #333;
}
.fetch-random-quote-button {
  margin-top: 20px;
  border: none;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  background-color: #76e29d;
  font-size: 15px;
  padding: 12px 20px;
  outline-color: #76e29d;
}
.fetch-random-quote-button:active {
  background-color: #63c787;
}
</style>
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
    wrapChuckNorrisContentInHtmlTag(string) {
      const wrapStringBetweenSpan = string =>
        `<span class='chuck-norris-name'>${string}</span>`;
      return string
        .replace(/Chuck Norris/g, wrapStringBetweenSpan("Chuck Norris"))
        .replace(/Chuck/g, wrapStringBetweenSpan("Chuck"));
    },
    saveQuote(quote) {
      this.quote = Object.assign(quote, {
        value: this.wrapChuckNorrisContentInHtmlTag(quote.value)
      });
      this.last_save = Date.now();
      this.hideLogin();
    }
  }
};
</script>
