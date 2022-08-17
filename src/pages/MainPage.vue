<template>
  <div class="container">
    <h1 id="main-title">LET'S GET COOKING!</h1>

    <b-container>
      <b-row>
        <b-col>
          <RecipePreviewList ref="random" title="Explore these recipes" class="RandomRecipes" />
          <b-button id="rand-button" v-on:click="$refs.random.updateRecipes()">More Recipes</b-button>
          <!-- <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
          {{ !$root.store.username }} -->
        </b-col>

        <b-col>
          <LoginPage v-if="!$root.store.username"></LoginPage>
          <RecipePreviewList v-else
            ref = "viewed"
            title="Last Viewed Recipes"
          ></RecipePreviewList>
        </b-col>
      </b-row>
    </b-container>
    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import LoginPage from "./LoginPage.vue";
export default {
  // data(){
  //   return{
  //     recipes:[
  //       {
  //         id: 641726,
  //         title: "Dulce De Leche Brownies",
  //         readyInMinutes: 45,
  //         image: "https://spoonacular.com/recipeImages/641726-556x370.jpg",
  //         popularity: 29,
  //         vegen: true,
  //         vegetarian: true,
  //         glutenFree: true
  //       },
  //       {
  //         id: 651389,
  //         title: "Medenjaci - Croatian Honey Spice Cookies",
  //         readyInMinutes: 45,
  //         image: "https://spoonacular.com/recipeImages/651389-556x370.jpg",
  //         popularity: 6,
  //         vegen: true,
  //         vegetarian: true,
  //         glutenFree: true
  //       }
  //     ]
  //   }
  // },
  components: {
    RecipePreviewList,
    LoginPage
  },
  mounted() {
    this.$refs.random.updateRecipes();
    this.$refs.viewed.updateViewedRecipes();
    // this.$refs.random.pushRecipes(this.recipes);
  },
};
</script>

<style lang="scss" scoped>
.container{
  text-align:center
}
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

#main-title{
  font-family: 'Corben', cursive;
  text-shadow: 2px 4px #000000;
  -webkit-text-stroke: 1px black;
  color: #fef5a2;
}

#rand-button{
  // display: block;
  width: 200px;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:#ebc2ce;
  border-style: solid;
  border-radius: 5px;
  font-weight: bold;
  color: rgb(5, 5, 5);
  transition-duration: 0.4s;
}

#rand-button:hover{
  background-color:#cc90a1;

}
</style>
