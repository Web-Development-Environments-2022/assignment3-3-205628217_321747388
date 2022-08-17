<template>
  <b-container>
    <h3 id="title-fam">
      {{ title }}
      <slot></slot>
    </h3>
    <b-row v-for="r in recipes" :key="r.id">
        <b-col>
            <div class="recipe-header mt-3 mb-4">
                <h1>{{ r.title }}</h1>
                <img :src="r.image" class="center" />
            </div>
            <div class="recipe-body">
                <div class="wrapper">
                    <div class="wrapped">
                        <div class="mb-3">
                            <div>Recipe by: {{r.recipeBy}}</div>
                            <div>When to have: {{r.whenToHave}}</div>
                        </div>
                        Ingredients:
                        <div style="white-space: pre-wrap;">{{r.ingredients}}</div>
                    </div>
                    <div class="wrapped">
                        Instructions:
                        <div style="white-space: pre-wrap;">{{r.instructions}}</div>
                    </div>
                </div>
            </div>
        </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
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
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/familyRecipes",
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

#title-fam{
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: 50px;
  text-align: center;
}
</style>
