<template>
  <div id="view-recipe" class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1 id="title-recipe">{{ recipe.title }}</h1>
        <img id="recipe-img" :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div id="recipe-details" class="mb-3">
          <div style="display: inline;">TOTAL TIME: {{ recipe.readyInMinutes}} MIN |</div>
          <div style="display: inline;"> {{ recipe.servings}} SERVINGS |</div>
          <div style="display: inline;"> {{ recipe.aggregateLikes }} LIKES |</div>
          <div v-if="vegan" style="display: inline;"> VEGAN |</div>
          <div v-if="vegetarian" style="display: inline;"> VEGETARIAN |</div>
          <div v-if="glutenFree" style="display: inline;"> GLUTEN FREE</div>
          <!-- <div v-if="$root.store.username && !myRecipe">
            <div>viewed</div>
            <div v-if="!favorite">
              <button v-on:click="markAsFavorite">favorite</button>
            </div>
            <div v-else>faorite</div>
          </div> -->
          <div id="icons" v-if="$root.store.username && !myRecipe">
            <b-icon-heart-fill class="h5 mb-2" v-if="favorite" variant="danger" style="display: inline;"></b-icon-heart-fill>
            <button title="Add To Favorite" id="fav-button" v-if="!favorite" v-on:click="markAsFavorite">
            <b-icon-heart class="h5 mb-2" variant="secondary"></b-icon-heart></button>
            <b-icon-eye-fill class="h5 mb-2" variant="secondary"></b-icon-eye-fill>
          </div>
        </div>
        <div class="wrapper">
          <div class="wrapped">

            <h3 id="ing-title">INGREDIENTS :</h3>
            <ul v-if="!myRecipe">
              <li
                v-for="(r, index) in recipe.extendedIngredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
            <div v-else style="white-space: pre-wrap;">{{recipe.extendedIngredients}}</div>
          </div>
          <div class="wrapped">
            <h3 id="ins-title">INSTRUCTIONS :</h3>
            <ol v-if="!myRecipe">
              <li v-for="s in recipe._instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
            <div v-else style="white-space: pre-wrap;">{{recipe.analyzedInstructions}}</div>
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
          
          console.log("response.daata", response.data);
          if (response.status !== 200) this.$router.replace("/NotFound");
        } catch (error) {
          console.log("error.response.status", error.response.status);
          this.$router.replace("/NotFound");
          return;
        }
        console.log(response.data)
        let {
          analyzedInstructions,
          instructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title,
          servings
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
          title,
          servings
        };
        // console.log("instructions:")
        // console.log(analyzedInstructions);
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
          title,
          servings
        } = response.data;

        let _recipe = {
          analyzedInstructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title,
          servings
        };

        this.recipe = _recipe;
      }

    } catch (error) {
      console.log(error);
    }
    
  },
  methods: {
    async markAsFavorite() {
      try {
        let recipeId = this.$route.params.recipeId;
        const response = await this.axios.post(
          this.$root.store.server_domain + "/users/favorites",
          {
            recipeId: recipeId
          }
        );
        console.log("response.status", response.status);
        this.$root.toast("Favorite", "The Recipe successfully saved as favorite", "success");
      } catch (error) {
        console.log(error);
      }
      this.updateFavoriteList();
      this.favorite = true;
    },
    async updateFavoriteList() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/favorites",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        console.log(response);
        const recipes = response.data;
        let recipes_list = [];
        recipes_list.push(...recipes);
        this.$root.store.updateFavoriteList(recipes_list);
      } catch (error) {
        console.log(error);
      }
    },
  } 
};
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
  margin: 10px;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}

#view-recipe{
  font-size: 20px;
  color:white;
  font-weight: bolder;

}

#title-recipe{
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: 50px;
  text-align: center;
}

#ing-title, #ins-title{
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  /* font-size: 50px; */
  /* text-align: center; */
}

#recipe-img{
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  border-style: solid;
}

#recipe-details{
  text-align: center;
}

#fav-button{
  border: none;
  background-color: transparent;
}

#icons > * {
  margin-right: 10px;
  margin-left: 10px;
  padding: 0;
}
/* .recipe-header{

} */
</style>
