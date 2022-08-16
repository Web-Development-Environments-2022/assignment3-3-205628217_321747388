<template>
  <div>
    <router-link
      :to="{ name: 'recipe', params: { recipeId: recipe.id } }"
      class="recipe-preview"
    >
      <div class="recipe-body">
        <!-- <img v-if="image_load" :src="recipe.image" class="recipe-image" /> -->
        <img :src="recipe.image" class="recipe-image" />
      </div>
      <!-- <br/> -->
      <div class="recipe-footer">
        <div :title="recipe.title" class="recipe-title">
          {{ recipe.title }}
        </div>
        <ul class="recipe-overview">
          <li>{{ recipe.readyInMinutes }} minutes</li>
          <li>{{ recipe.popularity }} likes</li>
          <li v-if="recipe.vegan">vegan</li>
          <li v-if="recipe.vegetarian">vegetarian</li>
          <li v-if="recipe.glutenFree">gluten free</li>
        </ul>
      </div>
    </router-link>
    <div v-if="viewed">
      viewed
    </div>
    <div v-if="!favorite">
      <button v-on:click="markAsFavorite">favorite</button>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    // this.axios.get(this.recipe.image).then((i) => {
    //   this.image_load = true;
    // });
    this.checkViewed();
    this.checkfavorite();
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
      // console.log(recipeId);
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
.recipe-preview > .recipe-body {
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
}
</style>
