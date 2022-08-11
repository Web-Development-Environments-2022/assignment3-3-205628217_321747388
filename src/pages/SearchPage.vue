<template>
  <div class="container">
    <h1 class="title">Search Page</h1>

    <b-form @submit.prevent="onSearch" @reset.prevent="onReset">
      <!-- Query -->
      <b-form-group
        id="search-query"
        label-cols-sm="3"
        label="Search:"
        label-for="search"
      >
        <b-form-input
          id="search"
          v-model="$v.form.search.$model"
          type="text"
          :state="validateState('search')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.search.required">
          Please provide query
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.search.length">
          Query shoult not be over 300 characters
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- Number of results -->
      <b-form-group
        id="number"
        label-cols-sm="3"
        label="Max number of results:"
        label-for="number"
      >
        <b-form-select
          id="number"
          v-model="$v.form.number.$model"
          :options="options"
          :state="validateState('number')"
        ></b-form-select>
      </b-form-group>

      
      <!-- Cuisine -->
      <b-form-group
        id="choose-cuisine"
        label-cols-sm="3"
        label="Cuisine:"
        label-for="cuisine"
      >
        <b-form-select
          id="cuisine"
          v-model="$v.form.cuisine.$model"
          :options="cuisines"
          :state="validateState('cuisine')"
        ></b-form-select>
      </b-form-group>

      <!-- Diet -->
      <b-form-group
        id="choose-diet"
        label-cols-sm="3"
        label="Diet:"
        label-for="diet"
      >
        <b-form-select
          id="diet"
          v-model="$v.form.diet.$model"
          :options="diets"
          :state="validateState('diet')"
        ></b-form-select>
      </b-form-group>

      <!-- Intolerances -->
      <b-form-group
        id="choose-intolerance"
        label-cols-sm="3"
        label="Intolerances:"
        label-for="intolerance"
      >
        <b-form-select
          id="intolerance"
          v-model="$v.form.intolerance.$model"
          :options="intolerances"
          :state="validateState('intolerance')"
        ></b-form-select>
      </b-form-group>


      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Search</b-button>
    </b-form>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Register failed: {{ form.submitError }}
    </b-alert>

    <!-- {{ search_results }} -->
    <!-- <b-col v-for="r in search_results" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
    </b-col> -->
    <RecipePreviewList ref="res" title="Explore this recipes" class="RandomRecipes center" />

  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
// import RecipePreview from "../components/RecipePreview.vue";
import {
  required,
  maxLength,
} from "vuelidate/lib/validators";
export default {
  name: "Search",
  components: {
  RecipePreviewList,
  // RecipePreview
},
  data() {
    return {
      form: {
        search: "",
        number: "5",
        cuisine: "",
        diet: "",
        intolerance:""
      },
      options: [{ value: "5", text: "5"}, { value: "10", text: "10"}, { value: "15", text: "15"}],

      cuisines: [{value:"", text: "No preference"}, {value:"African", text:"African"}, {value:"American", text:"American"}, {value:"British", text:"British"}, 
      {value:"Cajun", text:"Cajun"}, {value:"Caribbean", text:"Caribbean"}, {value:"Chinese", text:"Chinese"}, {value:"Eastern European", text:"Eastern European"},
      {value:"European", text:"European"}, {value:"French", text:"French"}, {value:"German", text:"German"}, {value:"Greek", text:"Greek"}, {value:"Indian", text:"Indian"},
      {value:"Irish", text:"Irish"}, {value:"Italian", text:"Italian"}, {value:"Japanese", text:"Japanese"}, {value:"Jewish", text:"Jewish"}, {value:"Korean", text:"Korean"},
      {value:"Latin American", text:"Latin American"}, {value:"Mediterranean", text:"Mediterranean"}, {value:"Mexican", text:"Mexican"}, {value:"Middle Eastern", text:"Middle Eastern"}, 
      {value:"Nordic", text:"Nordic"}, {value:"Southern", text:"Southern"}, {value:"Spanish", text:"Spanish"}, {value:"Thai", text:"Thai"}, {value:"Vietnamese", text:"Vietnamese"}],

      diets: [{value:"", text: "No preference"}, {value:"gluten free", text:"Gluten Free"}, {value:"ketogenic", text:"Ketogenic"}, {value:"vegetarian", text:"Vegetarian"}, 
      {value:"lacto-vegetarian", text:"Lacto-Vegetarian"}, {value:"ovo-vegetarian", text:"Ovo-Vegetarian"}, {value:"vegan", text:"Vegan"}, {value:"pescetarian", text:"Pescetarian"}, 
      {value:"paleo", text:"Paleo"}, {value:"primal", text:"Primal"}, {value:"low FODMAP", text:"Low FODMAP"}, {value:"whole30", text:"Whole30"}],

      intolerances: [{value:"", text: "No preference"}, {value:"Dairy", text: "Dairy"}, {value:"Egg", text: "Egg"}, {value:"Gluten", text: "Gluten"}, {value:"Grain", text: "Grain"}, 
      {value:"Peanut", text: "Peanut"}, {value:"Seafood", text: "Seafood"}, {value:"Sesame", text: "Sesame"}, {value:"Shellfish", text: "Shellfish"}, 
      {value:"Soy", text: "Soy"}, {value:"Sulfite", text: "Sulfite"}, {value:"Tree Nut", text: "Tree Nut"}, {value:"Wheat", text: "Wheat"}],

      errors: [],
      validated: false,
      search_results: []
    };
  },
  validations: {
    form: {
      search: {
        required,
        length: (u) => maxLength(300)(u),
      },
      number:{

      },
      cuisine:{

      },
      diet:{

      },
      intolerance:{

      }
    }
  },
  // mounted() {
  // },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      // console.log(this.form.search);
      // console.log(this.form.results);
      // console.log(this.form.cuisine);
      // console.log(this.form.diet);
      // console.log(this.form.intolerance);
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/search",
          {
            params:{
              searchQuery: this.form.search,
              num: this.form.number.toString(),
              cuisine: this.form.cuisine,
              diet: this.form.diet,
              intolerances: this.form.intolerance
            }
            
          }
        );
        this.search_results = response.data;
        this.$refs.res.pushRecipes(this.search_results);

        // this.$router.push("/login");
        console.log(response);
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onSearch() {
      console.log("Search method called");
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      console.log("Search method go");
      this.Search(this.form.search);
    },
    onReset() {
      this.form = {
        search: "",
        number: "5",
        cuisine: "",
        diet: "",
        intolerance: ""
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  }
};
</script>

<style>

</style>
