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
      <div id="recipe-details">
        TOTAL TIME: {{recipe.readyInMinutes}} MIN | {{ recipe.popularity }} LIKES
      </div>
      <div id="dietery">
        <div v-if="recipe.vegetarian" style="display: inline;">VEGETARIAN |</div>
        <div v-if="recipe.vegan" style="display: inline;"> VEGAN |</div>
        <div v-if="recipe.glutenFree" style="display: inline;"> GLUTEN FREE</div>
      </div>   
    </b-card-text>
    <!-- <div id="icons" v-if="$root.store.username" style="display: inline;">
      <div id="view-icons" style="display: inline;">
        <div v-if="viewed" style="display: inline;">
          <b-icon-eye-fill variant="secondary"></b-icon-eye-fill>
        </div>
        <div v-else style="display: inline;">
          <b-icon-eye variant="secondary"></b-icon-eye>
        </div>
      </div>
      <div id="favorite-icons" style="display: inline;">
        <div v-if="favorite">
          <b-icon-heart-fill variant="danger" style="display: inline;"></b-icon-heart-fill>
        </div>
        <div v-else>
          <button v-on:click="markAsFavorite" style="display: inline;">
          <b-icon-heart variant="secondary" style="display: inline;"></b-icon-heart></button>
        </div>
      </div> -->
    <div id="icons" v-if="$root.store.username" style="display: inline;">
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
    // async markAsFavorite() {
    //   try {
    //     let recipeId = this.recipe.id;
    //     const response = await this.axios.post(
    //       this.$root.store.server_domain + "/users/favorites",
    //       {                                                                                                                                                                                                                                    
    //         recipeId: recipeId
    //       }
    //     );
    //     console.log("response.status", response.status);
    //     this.$root.toast("Favorite", "The Recipe successfully saved as favorite", "success");
    //   } catch (error) {
    //     console.log(error);
    //   }
    //   this.updateFavoriteList();
    // },
    markAsFavorite(){
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
    checkViewed() {
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
}

#title{
  font-family: 'Corben', cursive;
  /* font-family: 'Mukta', sans-serif; */
  /* -webkit-text-stroke: 1px black; */
  font-style: italic;
  font-size: x-large;
  color: rgb(22, 22, 22);
}



/* .recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
} */


</style>
