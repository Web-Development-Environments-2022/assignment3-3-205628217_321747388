<template>
  <div class="container center">
    <FamilyRecipes
      ref="familyRecipes"
      title="Family Recipes"
      class="RandomRecipes center"
    />
  </div>
</template>

<script>
import FamilyRecipes from "../components/FamilyRecipes";
export default {
  components: {
    FamilyRecipes,
  },
  mounted() {
    this.$refs.familyRecipes.updateRecipes();
  },
};
</script>

<style lang="scss" scoped>
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}

#title-fam {
  font-family: "Corben", cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: 50px;
  text-align: center;
}
</style>
