<template>
<div>
  <h1 id="search-title" class="title">Search Page</h1>
  <div id="search-form" class="container">


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

    <div id="buttons-s">
      <b-button id="reset-button-s" type="reset" variant="danger">Reset</b-button>
      <b-button
        id="search-button"
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Search</b-button>
    </div>
    </b-form>
    </div>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Search failed: {{ form.submitError }}
    </b-alert>

    <div v-if="submitted">
      <h5 id="no-res" v-if="noResults">We couldn't find recipes to match your search</h5>
      <div v-else class="text-center">
        <b-dropdown id="sort" v-if="!isEmpty" text="Sort By" variant="outline-dark" class="m-2">
          <b-dropdown-item v-on:click="this.sortByPrepTime">Preperation Time</b-dropdown-item>
          <b-dropdown-item v-on:click="this.sortByPopularity">Popularity</b-dropdown-item>
        </b-dropdown>      
        <RecipePreviewList ref="res" title="Search Results" class="RandomRecipes center" />
      </div>
    </div>
    <!-- <div id="divvv" class="center">
              <b-dropdown id="sort" text="Sort By" class="m-md-2">
          <b-dropdown-item v-on:click="this.sortByPrepTime">Preperation Time</b-dropdown-item>
          <b-dropdown-item v-on:click="this.sortByPopularity">Popularity</b-dropdown-item>
        </b-dropdown>      
        <RecipePreviewList ref="res" title="Search Results" class="RandomRecipes center" />
    </div> -->

    
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
      search_results: [],
      noResults: false,
      submitted: false
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
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/recipes/search",
          {
            params:{
              searchQuery: this.form.search,
              num: this.form.number,
              cuisine: this.form.cuisine,
              diet: this.form.diet,
              intolerances: this.form.intolerance
            }
            
          }
        );
        this.search_results = response.data;
        // console.log("b4 sort call");
        console.log(this.search_results);
        // let sorted = this.sortByPrepTime();
        // let sorted = this.sortByPopularity();
        // let sorted = this.sortByPrepTime;
        // console.log(sorted);
        if(!this.isEmpty){
                  this.$refs.res.pushRecipes(this.search_results);
        }
        else{
          this.noResults = true;
          this.$refs.res.pushRecipes(this.search_results);

        }
        // console.log(response);

        // Set last search
        this.$root.store.setLastSearch(JSON.stringify({
          search: this.form.search,
          number: this.form.number,
          cuisine: this.form.cuisine,
          diet: this.form.diet,
          intolerance: this.form.intolerance,
          search_results: this.search_results
        }));
        // console.log(JSON.parse(this.$root.store.lastSearch));
        
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onSearch() {
      this.noResults = false;
      this.submitted = true;
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
        intolerance: "",
        noResults: false
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    },
    getLastSearch(){
      var searchData = JSON.parse(this.$root.store.lastSearch);
      this.form.search = searchData.search,
      this.form.number = searchData.number,
      this.form.cuisine = searchData.cuisine,
      this.form.diet = searchData.diet,
      this.form.intolerance = searchData.intolerance,
      this.search_results = searchData.search_results
    },
    sortByPrepTime(){
      // console.log("in sort");
      // console.log(this.search_results);
      // return this.search_results.sort((a,b) => {a.readyInMinutes - b.readyInMinutes})
      return this.$refs.res.pushRecipes(this.search_results.sort(function(a,b){
          return a.readyInMinutes - b.readyInMinutes;
        }))
    },
    sortByPopularity(){
      // return this.search_results.sort((a,b) => {b.popularity - a.popularity})
      return this.$refs.res.pushRecipes(this.search_results.sort(function(a,b){
          return b.popularity - a.popularity;
        }))

    }

  },
  mounted(){
    if(this.$root.store.username){
      if(this.$root.store.lastSearch){
          this.getLastSearch();
          this.$refs.res.pushRecipes(this.search_results);

        }
    }
  },
  computed: {
    isEmpty: ({ search_results }) => search_results.length === 0
  },  

};
</script>

<style>
 #search-form{
  max-width: 600px;
  padding: 15px;
  margin-top: 30px;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:bisque;
  border-style: solid;
  border-radius: 5px;
  font-size: 20px;
  font-weight: bolder;
 }

 #search-title{
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: 50px;
  text-align: center;
 }

 #buttons-s{
  text-align: center;
}

#reset-button-s{
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:#691a32;
  border-style: solid;
  border-radius: 5px;
  font-weight: 500;
  color: white;
  transition-duration: 0.4s;
  width:75 px;

}
#reset-button-s:hover{
  background-color:#843a4f;

}
#search-button{
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:#ebc2ce;
  border-style: solid;
  border-radius: 5px;
  font-weight: bold;
  color: rgb(5, 5, 5);
  transition-duration: 0.4s;

}
#search-button:hover{
  background-color:#cc90a1;
}

#no-res{
  text-align: center;
  font-size: 23px;
  font-weight: 600;
  color: white;
  margin-top: 20px;
}

/* #sort{
  text-align: center;
  margin-top: 20px;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:white;
  border-style: solid;
  border-radius: 5px;
  font-weight: bold;
  color: rgb(5, 5, 5);
} */

#sort{
  background-color:#ebc2ce;
  border-radius:4px;

}

</style>
