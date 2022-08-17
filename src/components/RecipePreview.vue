<template>
<div>
  <b-card
    id="card"
    tag="article"
    style="max-width: 20rem;"
    class="mb-2"
  >
    <router-link
      :to="{ name: 'recipe', params: { recipeId: recipe.id, favorite: favorite, vegan: recipe.vegan, vegetarian: recipe.vegetarian, glutenFree: recipe.glutenFree, myRecipe: myRecipe} }" class="recipe-preview">
    <b-card-img id="img" :src="recipe.image" img-alt="Image"></b-card-img>
    </router-link>
    <b-card-title id="title" :title="recipe.title"></b-card-title>
    <b-card-text>
      <div id="recipe-details" v-if="!myRecipe">
        TOTAL TIME: {{recipe.readyInMinutes}} MIN | {{ recipe.popularity }} LIKES
      </div>
      <div id="recipe-details" v-else>
        TOTAL TIME: {{recipe.readyInMinutes}} MIN | {{ recipe.aggregateLikes }} LIKES
      </div>
      <div id="dietery">
        <div v-if="recipe.vegetarian" style="display: inline;">VEGETARIAN |</div>
        <div v-if="recipe.vegan" style="display: inline;"> VEGAN |</div>
        <div v-if="recipe.glutenFree" style="display: inline;"> GLUTEN FREE</div>
      </div>   
    </b-card-text>
    <div id="icons" v-if="$root.store.username && !myRecipe" style="display: inline;">
      <b-icon-heart-fill class="h5 mb-2" v-if="favorite" variant="danger" style="display: inline;"></b-icon-heart-fill>
      <button title="Add To Favorite" id="fav-button" v-if="!favorite" v-on:click="markAsFavorite">
      <b-icon-heart class="h5 mb-2" variant="secondary"></b-icon-heart></button>
      <b-icon-eye-fill class="h5 mb-2" v-if="viewed" variant="secondary"></b-icon-eye-fill>
      <b-icon-eye class="h5 mb-2" v-if="!viewed" variant="secondary"></b-icon-eye>
    </div>
  </b-card>
</div>
</template>

<script>
export default {
  mounted() {
    // this.axios.get(this.recipe.image).then((i) => {
    //   this.image_load = true;
    // });
    if (this.$root.store.username) {
      this.checkViewed();
      this.checkfavorite();
    }
  },
  data() {
    return {
      // image_load: false,
      viewed: false,
      favorite: false
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    },
    myRecipe: {
      type: Boolean,
      required: false
    }

    // id: {
    //   type: Number,
    //   required: true
    // },
    // title: {
    //   type: String,
    //   required: true
    // },
    // readyInMinutes: {
    //   type: Number,
    //   required: true
    // },
    // image: {
    //   type: String,
    //   required: true
    // },
    // aggregateLikes: {
    //   type: Number,
    //   required: false,
    //   default() {
    //     return undefined;
    //   }
    // }
  },
  methods: {
    async markAsFavorite() {
      try {
        let recipeId = this.recipe.id;
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
    async updateViewedList() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/viewed",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        console.log(response);
        const recipes = response.data;
        let recipes_list = [];
        recipes_list.push(...recipes);
        this.$root.store.updateViewedList(recipes_list);
      } catch (error) {
        console.log(error);
      }
    },
    checkViewed() {
      // if (this.$root.store.viewed_list.length == 0) {
      //   this.updateViewedList();
      // }
      let recipeId = this.recipe.id;
      let viewed_list = this.$root.store.viewed_list;
      for (let i = 0; i < viewed_list.length; i++){
        if (recipeId == viewed_list[i].id) {
          this.viewed = true;
          break;
        }
      }
    },
    checkfavorite() {
      // if (this.$root.store.favorite_list.length == 0) {
      //   this.updateFavoriteList();
      // }
      let recipeId = this.recipe.id;
      let favorite_list = this.$root.store.favorite_list;
      for (let i = 0; i < favorite_list.length; i++){
        if (recipeId == favorite_list[i].id) {
          this.favorite = true;
          break;
        }
      }
    }
  }
};
</script>

<style scoped>
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
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

#card{
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:bisque;
  font-weight: 500;
  margin: auto;
  margin-top: 20px;
  max-width:400px;
}

#title{
  font-family: 'Corben', cursive;
  /* font-family: 'Mukta', sans-serif; */
  /* -webkit-text-stroke: 1px black; */
  font-style: italic;
  font-size: x-large;
  color: rgb(24, 24, 24);
}
</style>
