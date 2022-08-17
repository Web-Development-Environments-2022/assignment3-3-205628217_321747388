<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.aggregateLikes }} likes</div>
              <div v-if="vegan">vegan</div>
              <div v-if="vegetarian">vegetarian</div>
              <div v-if="glutenFree">glutenFree</div>
              <div v-if="$root.store.username && !myRecipe">
                <div>viewed</div>
                <div v-if="!favorite">
                  <button v-on:click="markAsFavorite">favorite</button>
                </div>
                <div v-else>faorite</div>
              </div>
            </div>
            Ingredients:
            <ul v-if="!myRecipe">
              <li
                v-for="(r, index) in recipe.extendedIngredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
            <div v-else>{{recipe.extendedIngredients}}</div>
          </div>
          <div class="wrapped">
            Instructions:
            <ol v-if="!myRecipe">
              <li v-for="s in recipe._instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
            <div v-else>{{recipe.analyzedInstructions}}</div>
          </div>
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null,
      favorite: false,
      vegan: false,
      vegetarian: false,
      glutenFree: false,
      myRecipe: false
    };
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;
      let id = this.$route.params.recipeId;
      this.favorite = this.$route.params.favorite;
      this.vegan = this.$route.params.vegan;
      this.vegetarian = this.$route.params.vegetarian;
      this.glutenFree = this.$route.params.glutenFree;
      this.myRecipe = this.$route.params.myRecipe;

      if (!this.myRecipe){
        try {
          response = await this.axios.get(
            // "https://test-for-3-2.herokuapp.com/recipes/info",
            this.$root.store.server_domain + "/recipes/info/" + id,
            {
              // params: { recipeId: this.$route.params.recipeId }
            }
          );
          
          console.log("response.status", response.status);
          if (response.status !== 200) this.$router.replace("/NotFound");
        } catch (error) {
          console.log("error.response.status", error.response.status);
          this.$router.replace("/NotFound");
          return;
        }
        // console.log(response.data)
        let {
          analyzedInstructions,
          instructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        } = response.data;

        let _instructions = analyzedInstructions
          .map((fstep) => {
            fstep.steps[0].step = fstep.name + fstep.steps[0].step;
            return fstep.steps;
          })
          .reduce((a, b) => [...a, ...b], []);

        let _recipe = {
          instructions,
          _instructions,
          analyzedInstructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        };

        this.recipe = _recipe;

        // viewed:
        if (this.$root.store.username) {
          try {
            let recipeId = this.$route.params.recipeId;
            const response = await this.axios.post(
              this.$root.store.server_domain + "/users/viewed",
              {
                recipeId: recipeId
              }
            );
            console.log("response.status", response.status);
          } catch (error) {
            console.log(error);
          }

          try {
            const response = await this.axios.get(
              this.$root.store.server_domain + "/users/viewed",
              // "https://test-for-3-2.herokuapp.com/recipes/random"
            );
            console.log(response);
            const recipes = response.data;
            let recipes_list = [];
            recipes_list.push(...recipes);
            // console.log(recipes_list);
            this.$root.store.updateViewedList(recipes_list);
          } catch (error) {
            console.log(error);
          }
        }
      // My Recipe
      } else {
        try {
          response = await this.axios.get(
            // "https://test-for-3-2.herokuapp.com/recipes/info",
            this.$root.store.server_domain + "/recipes/myRecipe/" + id,
            {
              // params: { recipeId: this.$route.params.recipeId }
            }
          );
          console.log("response.status", response.status);
          if (response.status !== 200) this.$router.replace("/NotFound");
        } catch (error) {
          console.log("error.response.status", error.response.status);
          this.$router.replace("/NotFound");
          return;
        }
        // console.log(response.data)
        let {
          analyzedInstructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        } = response.data;

        let _recipe = {
          analyzedInstructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        };

        this.recipe = _recipe;
      }

    } catch (error) {
      console.log(error);
    }
    
  }
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
