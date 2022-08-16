<template>
  <b-container>
    <h3>
      {{ title }}
      <slot></slot>
    </h3>
    <b-row v-for="r in recipes" :key="r.id">
      <b-col>
        <RecipePreview class="recipePreview" :recipe="r" :myRecipe="myRecipe" />
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
      recipes: [],
      myRecipe: false
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
        // console.log(this.recipes);
        this.myRecipe = false;
      } catch (error) {
        console.log(error);
      }
    },
    pushRecipes(array){
      this.recipes = [];
      this.recipes.push(...array);
    },

    // async updateViewedRecipes() {
    //   try {
    //     const response = await this.axios.get(
    //       this.$root.store.server_domain + "/users/viewed",
    //       // "https://test-for-3-2.herokuapp.com/recipes/random"
    //     );
    //     console.log(response);
    //     const recipes = response.data;
    //     this.recipes = [];
    //     this.recipes.push(...recipes);
    //     console.log(this.recipes);
    //   } catch (error) {
    //     console.log(error);
    //   }
    // },
    updateViewedRecipes() {
      try {
        let viewed_list = this.$root.store.viewed_list;
        this.recipes = [];
        if (viewed_list.length > 0) {
          this.recipes.push(viewed_list[0]);
        }
        if (viewed_list.length > 1) {
          this.recipes.push(viewed_list[1]);
        }
        if (viewed_list.length > 2) {
          this.recipes.push(viewed_list[2]);
        }
        this.myRecipe = false;
      } catch (error) {
        console.log(error)
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
        this.myRecipe = true;
      } catch (error) {
        console.log(error);
      }
    },

    // async updateFavoriteRecipes() {
    //   try {
    //     const response = await this.axios.get(
    //       this.$root.store.server_domain + "/users/favorites",
    //       // "https://test-for-3-2.herokuapp.com/recipes/random"
    //     );
    //     console.log(response);
    //     const recipes = response.data;
    //     this.recipes = [];
    //     this.recipes.push(...recipes);
    //     this.myRecipe = false;
    //   } catch (error) {
    //     console.log(error);
    //   }
    // }
    updateFavoriteRecipes() {
      try {
        let favorite_list = this.$root.store.favorite_list;
        this.recipes = [];
        this.recipes.push(...favorite_list);
        this.myRecipe = false;
      } catch (error) {
        console.log(error)
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}

h3{
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: x-large;
}
</style>
