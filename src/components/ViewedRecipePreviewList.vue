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
  name: "ViewedRecipePreviewList",
  components: {
    RecipePreview,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      recipes: [],
      myRecipe: false,
    };
  },
  mounted() {
    this.updateViewedRecipes();
  },
  methods: {
    async updateViewedRecipes() {
      try {
        if (this.$root.store.viewed_list.length == 0) {
          try {
            const response = await this.axios.get(
              this.$root.store.server_domain + "/users/viewed"
            );
            const recipes = response.data;
            let viewed_list = [];
            viewed_list.push(...recipes);
            this.$root.store.updateViewedList(viewed_list);
          } catch (error) {
            console.log(error);
          }
        }

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
        console.log(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}

h3 {
  font-family: "Corben", cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: xx-large;
  margin-top: 10px;
}
</style>
