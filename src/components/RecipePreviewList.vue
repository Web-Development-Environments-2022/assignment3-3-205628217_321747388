<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <b-row v-for="r in recipes" :key="r.id">
      <b-col>
      <!-- <b-col v-for="r in recipes" :key="r.id"> -->
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  // mounted() {
  //   this.updateRecipes();
  // },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/random",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
    pushRecipes(array){
      this.recipes = [];
      this.recipes.push(...array);
    },

    async updateViewedRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/viewed",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },

    async updateMyRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/myRecipes",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },

    async updateFavoriteRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/favorites",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }

  }

};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>
