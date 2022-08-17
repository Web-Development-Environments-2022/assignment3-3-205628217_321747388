<template>
  <div class="container">
    <b-modal
      id="recipe-modal"
      ref="recipe-modal"
      title="Add New Recipe"
      header-bg-variant="secondary"
      header-text-variant="light"
      body-bg-variant="secondary"
      hide-footer
      @show="resetModal"
      @hidden="resetModal"
      @hide="exitModal"
      modal-class="my-second-class"
    >
      <b-form id="new-form" @submit.prevent="onSubmit" @reset.prevent="onReset">
        <!-- Recipe Name -->
        <b-form-group
          id="input-group-name"
          label-cols-sm="3"
          label="Name:"
          label-for="name"
        >
          <b-form-input
            id="name"
            v-model="$v.form.name.$model"
            type="text"
            :state="validateState('name')"
          ></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.name.required">
            Name is required
          </b-form-invalid-feedback>
          <b-form-invalid-feedback v-else-if="!$v.form.name.length">
            Name max length should be 300 characters
          </b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.name.alpha">
            Name may only contain alphabetic characters
          </b-form-invalid-feedback>
        </b-form-group>

        <!-- Image -->
        <b-form-group
          id="input-group-image"
          label-cols-sm="3"
          label="Image URL:"
          label-for="image"
        >
          <b-form-input
            id="image"
            v-model="$v.form.image.$model"
            type="text"
            :state="validateState('image')"
          ></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.image.required">
            Image is required
          </b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.image.length">
            Image URL max length should be 300 characters
          </b-form-invalid-feedback>
        </b-form-group>

        <!-- Ready in minutes -->
        <b-form-group
          id="input-group-time"
          label-cols-sm="3"
          label="Ready in minutes:"
          label-for="readyInMinutes"
        >
          <b-form-input
            id="readyInMinutes"
            v-model="$v.form.readyInMinutes.$model"
            type="number"
            :state="validateState('readyInMinutes')"
          ></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.required">
            Preperation time is required
          </b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.readyInMinutes.numeric">
            Preperation time can only be numeric
          </b-form-invalid-feedback>
        </b-form-group>

        <!-- Vegetarian -->
        <b-form-group
          id="input-group-vegetarian"
          label-cols-sm="3"
          label="Vegetarian:"
          label-for="vegetarian"
        >
          <b-form-select
            id="vegetarian"
            v-model="$v.form.vegetarian.$model"
            :options="options"
            :state="validateState('vegetarian')"
          ></b-form-select>
          <b-form-invalid-feedback>
            Please note if vegetarian
          </b-form-invalid-feedback>
        </b-form-group>

        <!-- Vegan -->
        <b-form-group
          id="input-group-vegan"
          label-cols-sm="3"
          label="Vegan:"
          label-for="vegan"
        >
          <b-form-select
            id="vegan"
            v-model="$v.form.vegan.$model"
            :options="options"
            :state="validateState('vegan')"
          ></b-form-select>
          <b-form-invalid-feedback>
            Please note if vegan
          </b-form-invalid-feedback>
        </b-form-group>

        <!-- Gluten Free -->
        <b-form-group
          id="input-group-glutenFree"
          label-cols-sm="3"
          label="Gluten Free:"
          label-for="glutenFree"
        >
          <b-form-select
            id="glutenFree"
            v-model="$v.form.glutenFree.$model"
            :options="options"
            :state="validateState('glutenFree')"
          ></b-form-select>
          <b-form-invalid-feedback>
            Please note if gluten free
          </b-form-invalid-feedback>
        </b-form-group>

        <!-- Ingredients -->
        <b-form-group
          id="input-group-ingredients"
          label-cols-sm="3"
          label="Ingredients:"
          label-for="ingredients"
        >
          <div v-for="(ing, index) in ingList" :key="index">
            <b-form-input
              id="ingredients"
              v-model="ing.value"
              type="text"
              :state="validateState('ingredients')"
            ></b-form-input>
          </div>
          <b-form-invalid-feedback v-if="!$v.form.ingredients.required">
            Ingredients is required
          </b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.ingredients.length">
            Ingredients max length should be 1000 characters
          </b-form-invalid-feedback>
        </b-form-group>
        <b-button id="add-ing" @click="addIng" class="ml-5 w-75">Add Ingredient</b-button>

        <!-- Instructions -->
        <b-form-group
          id="input-group-instructions"
          label-cols-sm="3"
          label="Instructions:"
          label-for="instructions"
        >
          <div v-for="(find, index) in finds" :key="index">
            <b-form-input
              id="instructions"
              v-model="find.value"
              type="text"
              :state="validateState('instructions')"
            ></b-form-input>
          </div>
          <b-form-invalid-feedback v-if="!$v.form.instructions.required">
            Instructions is required
          </b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.instructions.length">
            Instructions max length should be 2000 characters
          </b-form-invalid-feedback>
        </b-form-group>
        <b-button id="add-ins" @click="addFind" class="ml-5 w-75">Add Instruction</b-button>
        <!-- Servings -->
        <b-form-group
          id="input-group-servings"
          label-cols-sm="3"
          label="Servings:"
          label-for="servings"
        >
          <b-form-input
            id="servings"
            v-model="$v.form.servings.$model"
            type="number"
            :state="validateState('servings')"
          ></b-form-input>
          <b-form-invalid-feedback v-if="!$v.form.servings.required">
            Number of servings is required
          </b-form-invalid-feedback>
          <b-form-invalid-feedback v-if="!$v.form.servings.numeric">
            Number of servings can only be numeric
          </b-form-invalid-feedback>
        </b-form-group>

        <div id="buttons">
          <b-button id="reset-button" type="reset" variant="danger"
            >Reset</b-button
          >
          <b-button
            id="submit-button"
            type="submit"
            variant="primary"
            style="width:250px;"
            class="ml-5 w-75"
            >Submit</b-button
          >
        </div>
      </b-form>
      <b-alert
        class="mt-2"
        v-if="form.submitError"
        variant="warning"
        dismissible
        show
      >
        Submission failed: {{ form.submitError }}
      </b-alert>
    </b-modal>
  </div>
</template>

<script>
import { required, maxLength, alpha, numeric } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      form: {
        name: "",
        image: "",
        readyInMinutes: "",
        vegetarian: null,
        vegan: null,
        glutenFree: null,
        ingredients: "",
        instructions: "",
        servings: "",
        submitError: undefined,
      },
      options: [
        { value: "1", text: "Yes" },
        { value: "0", text: "No" },
      ],
      errors: [],
      validated: false,
      finds: [{ value: "" }],
      ingList: [{ value: "" }],
    };
  },
  validations: {
    form: {
      name: {
        required,
        length: (u) => maxLength(300)(u),
        alpha,
      },
      image: {
        required,
        length: (u) => maxLength(300)(u),
      },
      readyInMinutes: {
        required,
        numeric,
      },
      vegetarian: {
        required,
      },
      vegan: {
        required,
      },
      glutenFree: {
        required,
      },
      ingredients: {

      },
      instructions: {

      },
      servings: {
        required,
        numeric,
      },
    },
  },
  mounted() {
    this.$bvModal.show("recipe-modal");
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Submit() {
      try {
        const response = await this.axios.post(
          this.$root.store.server_domain + "/recipes/createRecipe",
          {
            title: this.form.name,
            readyInMinutes: this.form.readyInMinutes,
            image: this.form.image,
            vegan: this.form.vegan,
            vegetarian: this.form.vegetarian,
            glutenFree: this.form.glutenFree,
            extendedIngredients: this.form.ingredients,
            analyzedInstructions: this.form.instructions,
            servings: this.form.servings,
          }
        );
        this.$bvModal.hide("add-recipe-modal");
        this.$root.toast("Add Recipe", "Recipe added", "success");
        this.$router.back();
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onSubmit() {
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      this.form.instructions = this.prepareInstruction();
      this.form.ingredients = this.prepareIngredients();
      this.Submit();
    },
    onReset() {
      this.form = {
        name: "",
        image: "",
        readyInMinutes: "",
        vegetarian: null,
        vegan: null,
        glutenFree: null,
        ingredients: "",
        instructions: "",
        servings: "",
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    },
    resetModal() {
      this.onReset();
    },
    exitModal(bvModalEvt) {
      if (
        confirm("Your progress will be lost. Are you sure you want to leave?")
      ) {
        this.$router.back();
      } else {
        bvModalEvt.preventDefault();
      }
    },
    addFind: function() {
      this.finds.push({ value: "" });
    },
    addIng: function() {
      this.ingList.push({ value: "" });
    },
    prepareInstruction() {
      let newInstruction = "";
      for (let i = 0; i < this.finds.length; i++) {
        newInstruction += i + 1 + ". " + this.finds[i].value + "\n";
      }
      return newInstruction;
    },
    prepareIngredients() {
      let newIngredients = "";
      for (let j = 0; j < this.ingList.length; j++) {
        newIngredients += this.ingList[j].value + "\n";
      }
      return newIngredients;
    },
  },
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 500px;
}

#new-form {
  // border-color: rgba(5, 5, 5, 0.849);
  // border-width: 1px;
  // background-color:bisque;
  // border-style: solid;
  // border-radius: 5px;
  font-size: 20px;
  font-weight: 500;
  font-family: "Mukta", sans-serif;
  margin: 0%;
  color: white;
}

#name,
#image,
#readyInMinutes,
#vegetarian,
#vegan,
#glutenFree,
#ingredients,
#instructions,
#servings {
  border-style: solid;
  border-radius: 5px;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
}

#buttons {
  text-align: center;
}

#reset-button {
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color: #691a32;
  border-style: solid;
  border-radius: 5px;
  font-weight: 500;
  color: white;
  transition-duration: 0.4s;
  width: 75 px;
}
#reset-button:hover {
  background-color: #843a4f;
}
#submit-button {
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color: #cc90a1;
  border-style: solid;
  border-radius: 5px;
  font-weight: 500;
  color: white;
  transition-duration: 0.4s;
}
#submit-button:hover {
  background-color: #843a4f;
}

#add-ing, #add-ins{
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color: #daccd0;
  border-style: solid;
  border-radius: 5px;
  font-weight: 500;
  color: black;
  transition-duration: 0.4s;
  width: 30 px;
  margin-bottom: 10px;
}
</style>
