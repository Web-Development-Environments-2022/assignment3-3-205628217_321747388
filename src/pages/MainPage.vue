<template>
  <div class="container">
    <h1 class="title">Main Page</h1>

    <b-container>
      <b-row>
        <b-col>
          <RecipePreviewList ref="random" title="Explore this recipes" class="RandomRecipes center" />
          <b-button v-on:click="$refs.random.updateRecipes()">More</b-button>
          <!-- <router-link v-if="!$root.store.username" to="/login" tag="button">You need to Login to vue this</router-link>
          {{ !$root.store.username }} -->
        </b-col>

        <b-col>
          <LoginPage v-if="!$root.store.username"></LoginPage>
          <RecipePreviewList v-else
            title="Last Viewed Recipes"
            :class="{
              RandomRecipes: true,
              blur: !$root.store.username,
              center: true
            }"
            disabled
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
  components: {
    RecipePreviewList,
    LoginPage
}
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
