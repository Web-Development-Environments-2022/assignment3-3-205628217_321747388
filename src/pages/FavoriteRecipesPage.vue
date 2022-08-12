<template>
  <div class="container">
    <h1 class="title">Favorite Recipes</h1>
    
    <RecipePreviewList ref="favoriteRecipes" title="Favorite recipes" class="RandomRecipes center" />
             
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  components: {
    RecipePreviewList
  },
  mounted() {
    this.$refs.favoriteRecipes.updateFavoriteRecipes();//*** */
  },
};
</script>

<style lang="scss" scoped>
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
</style>
