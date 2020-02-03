<template>
  <div class="homepage">
    <QuoteOfTheDay :fetchedQuote="quote" />
    <hr />
    <CategoryList @random_quote_from_category="value => quote = value" />
    <hr />
    <SearchModule />
  </div>
</template>

<script>
import QuoteOfTheDay from "./QuoteOfTheDay";
import CategoryList from "./CategoryList";
import SearchModule from "./SearchModule";

export default {
  name: "HomePage",
  components: { CategoryList, QuoteOfTheDay, SearchModule },
  data() {
    return {
      quote: {}
    };
  },
  mounted() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
