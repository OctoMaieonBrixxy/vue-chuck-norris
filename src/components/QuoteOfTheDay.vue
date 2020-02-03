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
    >Another one</button>
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

  props: {
    fetchedQuote: {
      type: Object,
      required: true,
      default: () => ({})
    }
  },

  data() {
    return {
      loading: true,
      quote: {},
      last_save: null
    };
  },

  watch: {
    fetchedQuote(newValue) {
      this.saveQuote(newValue);
    }
  },

  mounted() {
    this.fetchRandomQuote();
  },

  methods: {
    hideLoading() {
      clearTimeout(this.timeoutId);
      this.loading = false;
    },
    showLoading() {
      this.timeoutId = setTimeout(() => {
        this.loading = true;
      }, TIMEOUT);
    },
    fetchRandomQuote() {
      this.showLoading();
      fetch("https://api.chucknorris.io/jokes/random")
        .then(response => response.json())
        .then(quote => {
          this.hideLoading();
          this.saveQuote(quote);
        });
    },
    wrapChuckNorrisContentInHtmlTag(string) {
      const wrapStringBetweenSpan = string =>
        `<span class='chuck-norris-name'>${string}</span>`;
      return (
        string &&
        string
          .replace(/Chuck Norris/g, wrapStringBetweenSpan("Chuck Norris"))
          .replace(/Chuck/g, wrapStringBetweenSpan("Chuck"))
      );
    },
    saveQuote(quote) {
      this.quote = Object.assign(quote, {
        value: this.wrapChuckNorrisContentInHtmlTag(quote.value)
      });
      this.last_save = Date.now();
      this.hideLoading();
    }
  }
};
</script>
