<template>
  <div class="category">
    <button
      v-for="category in category_list"
      v-bind:key="category"
      v-on:click="getCategory(category)"
      class="category-item"
      v-bind:class="{ [getRandomClass()]: true }"
    >{{category}}</button>
  </div>
</template>


<style scoped>
.category-item {
  width: 100px;
  height: 100px;
}
</style>
<script>
export default {
  name: "CategoryList",
  data() {
    return {
      category_list: []
    };
  },
  mounted() {
    this.fetchAllCategories();
  },
  methods: {
    getRandomClass() {
      function random(min, max) {
        return Math.floor(min + Math.random() * (max - min));
      }

      const gradient_class_list = ["turquoise-g", "blue-g"];
      let random_int = random(0, gradient_class_list.length - 1);

      return gradient_class_list[random_int];
    },
    fetchAllCategories() {
      fetch("https://api.chucknorris.io/jokes/categories")
        .then(response => response.json())
        .then(categories => {
          this.category_list = categories;
        });
    },
    getCategory(category) {
      fetch("https://api.chucknorris.io/jokes/random?category=" + category)
        .then(response => response.json())
        .then(random_quote_from_category => {
          this.$emit("random_quote_from_category", random_quote_from_category);
        });
    }
  }
};
</script>