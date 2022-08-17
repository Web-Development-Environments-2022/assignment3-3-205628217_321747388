<template>
  <div id="family-recipe" class="container">
    <h3 id="title-fam">
      {{ title }}
      <slot></slot>
    </h3>
    <b-row id="fam-recipe" v-for="r in recipes" :key="r.id">
        <b-col>
            <div class="recipe-header mt-3 mb-4">
              <h1 id="title-recipe">{{ r.title }}</h1>
                <img id="recipe-img" :src="r.image" class="center" />
            </div>
            <div class="recipe-body">
              <div id="recipe-details" class="mb-3" style="text-align:center">
                  <div >{{r.recipeBy}}'s Recipe</div>
                  <div>We usually have it on {{r.whenToHave}}</div>
              </div>
                <div class="wrapper">
                    <div class="wrapped">
                        
                      <h3 id="ing-title">Ingredients:</h3>
                        <div style="white-space: pre-wrap;">{{r.ingredients}}</div>
                    </div>
                    <div class="wrapped">
                        <h3 id="ins-title">Instructions:</h3>
                        <div style="white-space: pre-wrap;">{{r.instructions}}</div>
                    </div>
                </div>
            </div>
        </b-col>
    </b-row>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/familyRecipes",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );

        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
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
  width: 70%;
}
// .container {
//   min-height: 400px;
// }
#fam-recipe{
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  // background-color:bisque;
  background-image: url(https://img.freepik.com/premium-photo/sheet-blank-bright-yellow-lined-notebook-paper-background-extra-large-highly-detailed-image_598586-199.jpg?w=2000);
  background-size: cover;
  border-style: solid;
  border-radius: 5px;
  margin-bottom: 20px;
  // max-width: 600px;
}

#title-fam{
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: 50px;
  text-align: center;
}

#recipe-img{
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  border-style: solid;
}

#title-recipe{
  // font-family: 'Corben', cursive;
  // text-shadow: 2px 3.5px #000000;
  // -webkit-text-stroke: 1.2px black;
  color: #561283;
  font-size: 35px;
  text-align: center;
  font-weight: bold;
  text-decoration: underline;
  text-decoration-style: wavy;
}

#family-recipe{
  font-family: 'Handlee', cursive;
  font-size: 20px;
  font-weight: bold;
  color: #293d87;
}

#recipe-details{
  font-size: 25px;
}

#ing-title, #ins-title{
  // color: #561283;
  font-size: 25px;
  // text-align: center;
  font-weight: bold;
  text-decoration: underline;
  text-decoration-style: wavy;
}
</style>
