<template>
  <div id="reg-form" class="container">
    <h1 id="reg-title" class="title">Register</h1>
    <br/>
    <b-form @submit.prevent="onRegister" @reset.prevent="onReset">
      <!-- username -->
      <b-form-group
        id="input-group-username"
        label-cols-sm="3"
        label="Username:"
        label-for="username"
      >
        <b-form-input
          id="username"
          v-model="$v.form.username.$model"
          type="text"
          :state="validateState('username')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.username.required">
          Username is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.username.length">
          Username length should be between 3-8 characters long
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.form.username.alpha">
          Username may only contain alphabetic characters
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- first name -->
      <b-form-group
        id="input-group-firstname"
        label-cols-sm="3"
        label="First Name:"
        label-for="firstName"
      >
        <b-form-input
          id="firstName"
          v-model="$v.form.firstName.$model"
          type="text"
          :state="validateState('firstName')" 
        ></b-form-input>
        <!-- <b-form-invalid-feedback v-if="!$v.form.firstName.required">
          Username is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.username.length">
          Username length should be between 3-8 characters long
        </b-form-invalid-feedback> -->
        <b-form-invalid-feedback v-if="!$v.form.firstName.alpha">
          First name may only contain alphabetic characters
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- last name -->
      <b-form-group
        id="input-group-lastname"
        label-cols-sm="3"
        label="Last Name:"
        label-for="lastName"
      >
        <b-form-input
          id="lastName"
          v-model="$v.form.lastName.$model"
          type="text"
          :state="validateState('lastName')" 
        ></b-form-input>
        <!-- <b-form-invalid-feedback v-if="!$v.form.firstName.required">
          Username is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-else-if="!$v.form.username.length">
          Username length should be between 3-8 characters long
        </b-form-invalid-feedback> -->
        <b-form-invalid-feedback v-if="!$v.form.lastName.alpha">
          Last name may only contain alphabetic characters
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- country -->
      <b-form-group
        id="input-group-country"
        label-cols-sm="3"
        label="Country:"
        label-for="country"
      >
        <b-form-select
          id="country"
          v-model="$v.form.country.$model"
          :options="countries"
          :state="validateState('country')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Country is required
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- password -->
      <b-form-group
        id="input-group-Password"
        label-cols-sm="3"
        label="Password:"
        label-for="password"
      >
        <b-form-input
          id="password"
          type="password"
          v-model="$v.form.password.$model"
          :state="validateState('password')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.password.required">
          Password is required
        </b-form-invalid-feedback>
        <b-form-text v-else-if="$v.form.password.$error" text-variant="info">
          Your password should be <strong>strong</strong>. <br />
          For that, your password should also:
        </b-form-text>
        <b-form-invalid-feedback
          v-if="$v.form.password.required && !$v.form.password.length">
          Have length between 5-10 characters long
        </b-form-invalid-feedback>
        <b-form-invalid-feedback
          v-if="$v.form.password.required && !$v.form.password.containsNumber">
          Contain at least one number
        </b-form-invalid-feedback>
        <b-form-invalid-feedback
          v-if="$v.form.password.required && !$v.form.password.containsSpecial">
          Contain at least one specail character (#,?,!,@,$,%,^,&,*,-)
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- confirm password -->
      <b-form-group
        id="input-group-confirmedPassword"
        label-cols-sm="3"
        label="Confirm Password:"
        label-for="confirmedPassword"
      >
        <b-form-input
          id="confirmedPassword"
          type="password"
          v-model="$v.form.confirmedPassword.$model"
          :state="validateState('confirmedPassword')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.confirmedPassword.required">
          Password confirmation is required
        </b-form-invalid-feedback>
        <b-form-invalid-feedback
          v-else-if="!$v.form.confirmedPassword.sameAsPassword"
        >
          The confirmed password is not equal to the original password
        </b-form-invalid-feedback>
      </b-form-group>

      <!-- email -->
      <b-form-group
        id="input-group-Password"
        label-cols-sm="3"
        label="E-mail:"
        label-for="email"
      >
        <b-form-input
          id="email"
          type="email"
          v-model="$v.form.email.$model"
          :state="validateState('email')"
        ></b-form-input>
       <b-form-invalid-feedback v-if="!$v.form.email.email">
          Please enter valid E-mail
        </b-form-invalid-feedback>
         <!-- <b-form-text v-else-if="$v.form.password.$error" text-variant="info">
          Your password should be <strong>strong</strong>. <br />
          For that, your password should also:
        </b-form-text>
        <b-form-invalid-feedback
          v-if="$v.form.password.required && !$v.form.password.length">
          Have length between 5-10 characters long
        </b-form-invalid-feedback> -->
      </b-form-group>
      <div id="buttons">
      <b-button id="reset-button" type="reset" variant="danger">Reset</b-button>
      <b-button
        id="reg-button"
        type="submit"
        variant="primary"
        class="ml-5 w-75"
        >Register</b-button
      >
      </div>
      <div class="mt-2">
        You have an account already?
        <router-link to="login"> Log in here</router-link>
      </div>
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
    <!-- <b-card class="mt-3 md-3" header="Form Data Result">
      <pre class="m-0"><strong>form:</strong> {{ form }}</pre>
      <pre class="m-0"><strong>$v.form:</strong> {{ $v.form }}</pre>
    </b-card> -->
  </div>
</template>

<script>
import countries from "../assets/countries";
import {
  required,
  minLength,
  maxLength,
  alpha,
  sameAs,
  email
} from "vuelidate/lib/validators";

export default {
  name: "Register",
  data() {
    return {
      form: {
        username: "",
        firstName: "",
        lastName: "",
        country: null,
        password: "",
        confirmedPassword: "",
        email: "",
        submitError: undefined
      },
      countries: [{ value: null, text: "", disabled: true }],
      errors: [],
      validated: false
    };
  },
  validations: {
    form: {
      username: {
        required,
        length: (u) => minLength(3)(u) && maxLength(8)(u),
        alpha
      },
      firstName: {
        alpha
      },
      lastName: {
        alpha
      },
      country: {
        required
      },
      password: {
        required,
        length: (p) => minLength(5)(p) && maxLength(10)(p),
        containsNumber: function(p) {
          return /[0-9]/.test(p)
        },
        containsSpecial: function(p) {
          return /[#?!@$%^&*-]/.test(p)
        }
      },
      confirmedPassword: {
        required,
        sameAsPassword: sameAs("password"),
      }, 
      email:
      {
        email
      }
    }
  },
  mounted() {
    // console.log("mounted");
    this.countries.push(...countries);
    // console.log($v);
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Register() {
      try {
        const response = await this.axios.post(
          // "https://test-for-3-2.herokuapp.com/user/Register",
          this.$root.store.server_domain + "/Register",

          {
            username: this.form.username,
            password: this.form.password
          }
        );
        this.$router.push("/login");
        console.log(response);
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }
    },
    onRegister() {
      console.log("register method called");
      this.$v.form.$touch();
      if (this.$v.form.$anyError) {
        return;
      }
      console.log("register method go");
      this.Register();
    },
    onReset() {
      this.form = {
        username: "",
        firstName: "",
        lastName: "",
        country: null,
        password: "",
        confirmedPassword: "",
        email: ""
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  }
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 600px;
}
#reg-form{
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

#reg-title{
  font-family: 'Corben', cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: 50px;
  text-align: center;
}

#buttons{
  text-align: center;
}

#reset-button{
  // display: block;
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
#reset-button:hover{
  background-color:#843a4f;

}
#reg-button{
  // display: block;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color:#ebc2ce;
  border-style: solid;
  border-radius: 5px;
  font-weight: bold;
  color: rgb(5, 5, 5);
  transition-duration: 0.4s;

}
#reg-button:hover{
  background-color:#cc90a1;

}
</style>
