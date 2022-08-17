<template>
  <div style="text-align: center;">
    <h3 id="login-title">
        Join the community!
        <slot></slot>
    </h3>
    <div id="login-form" class="container">
      <h1 id="title">Login</h1>
      <b-form id="login-form" @submit.prevent="onLogin">
        <b-form-group
          id="input-group-Username"
          label-cols-sm="3"
          label="USERNAME :"
          label-for="Username"
        >
          <b-form-input
            id="Username"
            v-model="$v.form.username.$model"
            type="text"
            :state="validateState('username')"
          ></b-form-input>
          <b-form-invalid-feedback>
            Username is required
          </b-form-invalid-feedback>
        </b-form-group>

        <b-form-group
          id="input-group-Password"
          label-cols-sm="3"
          label="PASSWORD :"
          label-for="Password"
        >
          <b-form-input
            id="Password"
            type="password"
            v-model="$v.form.password.$model"
            :state="validateState('password')"
          ></b-form-input>
          <b-form-invalid-feedback>
            Password is required
          </b-form-invalid-feedback>
        </b-form-group>

        <b-button
          id="login-button"
          type="submit"
          variant="primary"
          class="mx-auto w-50"
          >LOGIN</b-button
        >
        <div class="mt-2">
          <!-- Do not have an account yet? -->
          DON'T HAVE AN ACCOUNT YET?
          <router-link to="register" style="color:steelblue"> REGISTER HERE</router-link>
        </div>
      </b-form>
      <b-alert
        class="mt-2"
        v-if="form.submitError"
        variant="warning"
        dismissible
        show
      >
        Login failed: {{ form.submitError }}
      </b-alert>
      <!-- <b-card class="mt-3" header="Form Data Result">
        <pre class="m-0">{{ form }}</pre>
      </b-card> -->
    </div>
  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";
export default {
  name: "Login",
  data() {
    return {
      form: {
        username: "",
        password: "",
        submitError: undefined
      }
    };
  },
  validations: {
    form: {
      username: {
        required
      },
      password: {
        required
      }
    }
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Login() {
      try {
        
        const response = await this.axios.post(
          // "https://test-for-3-2.herokuapp.com/user/Login",
          this.$root.store.server_domain +"/Login",
          // "http://132.72.65.211:80/Login",
          // "http://132.73.84.100:80/Login",

          {
            username: this.form.username,
            password: this.form.password
          }
        );
        // console.log(response);
        // this.$root.loggedIn = true;
        // console.log(this.$root.store.login);
        this.$root.store.login(this.form.username);
        this.$router.push("/");
        this.updateViewed();
        this.updateFavorite();
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
      
    },
    onLogin() {
      // console.log("login method called");
      this.form.submitError = undefined;
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      // console.log("login method go");

      this.Login();

    },
    async updateViewed() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/viewed",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        this.$root.store.updateViewedList(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
    async updateFavorite() {
      try {
        const response = await this.axios.get(
          this.$root.store.server_domain + "/users/favorites",
          // "https://test-for-3-2.herokuapp.com/recipes/random"
        );
        console.log(response);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(...recipes);
        console.log(this.recipes);
        this.$root.store.updateFavoriteList(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
  padding: 15px;
  margin-top: 30px;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:bisque;
  border-style: solid;
  border-radius: 5px;
}

#title{
  font-family: 'Corben', cursive;
  font-style: italic;
  font-size: x-large;
  color: white;
  -webkit-text-stroke: 1.5px black;

}

#login-title {
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: x-large;
}

#login-form{
  font-family: 'Mukta', sans-serif;
}

#login-button{
  display: block;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:#ebc2ce;
  border-style: solid;
  border-radius: 5px;
  font-weight: bold;
  color: rgb(5, 5, 5);
  transition-duration: 0.4s;

}

#login-button:hover{
  background-color:#cc90a1;

}
</style>
