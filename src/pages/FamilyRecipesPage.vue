<template>
  <div class="container">
    <h1 class="title">Family Recipes</h1>
    
    <FamilyRecipes ref="familyRecipes" title="family recipes" class="RandomRecipes center" />
             
  </div>
</template>

<script>
import FamilyRecipes from "../components/FamilyRecipes";
export default {
  components: {
    FamilyRecipes
  },
  mounted() {
    this.$refs.familyRecipes.updateRecipes();
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
