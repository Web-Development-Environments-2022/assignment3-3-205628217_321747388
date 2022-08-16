<template>
  <div id="app">
    <div>
      <b-navbar id="nav" toggleable="md" sticky>
        <b-navbar-brand :to="{ name: 'main' }"><span  id="main">A & D'S RECIPES</span></b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item class="nav-item" :to="{ name: 'search' }">Search</b-nav-item>
            <b-nav-item class="nav-item" :to="{ name: 'about' }">About</b-nav-item>
          </b-navbar-nav>

          <b-navbar-nav class="ml-auto" v-if="!$root.store.username">
            <b-nav-text id="hello">Hello Guest!</b-nav-text>
            <b-nav-item :to="{ name: 'register' }">Register</b-nav-item>
            <b-nav-item :to="{ name: 'login' }">Login</b-nav-item>
          </b-navbar-nav>
          <b-navbar-nav class="ml-auto" v-else>
            <b-nav-text id="hello">Hello {{ $root.store.username }}!</b-nav-text>
            <!-- <b-nav-item :to="{ name: 'addRecipe' }">Add Recipe</b-nav-item> -->
            <!-- <b-button v-b-modal.modal-1>Add Recipe</b-button> -->
            <!-- <router-link tag="li" :to="{name: 'addRecipe'}" id="modal-link" class="nav-link">Add Recipe</router-link> -->
            <b-nav-item :to="{name: 'addRecipe'}" id="modal-link" class="nav-link">Add Recipe</b-nav-item>
            <b-nav-item-dropdown text="Personal Recipes" right>
              <b-dropdown-item variant="secondary" :to="{ name: 'favorites' }">Favorites</b-dropdown-item>
              <b-dropdown-item variant="secondary" :to="{ name: 'myRecipes' }">My Recipes</b-dropdown-item>
              <b-dropdown-item variant="secondary" :to="{ name: 'familyRecipes' }">Family Recipes</b-dropdown-item>
            </b-nav-item-dropdown>
            <b-nav-item @click="Logout">Logout</b-nav-item>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    async Logout() {
      try {
        
        const response = await this.axios.post(
          this.$root.store.server_domain +"/Logout"
        );
        // console.log(response);
      } catch (err) {
        console.log(err.response);
        this.form.submitError = err.response.data.message;
      }

      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";
@import url('https://fonts.googleapis.com/css2?family=Corben:wght@700&family=Mukta&display=swap');

#app {
  // font-family: Avenir, Helvetica, Arial, sans-serif;
  font-family: 'Mukta', sans-serif;
  // font-family: 'Corben', cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // color: #50392c;
  min-height: 100vh;
  background-color: #7591ae;
}

#nav {
  padding: 7px;
  // font-family: 'Mukta', sans-serif;
  // font-family: 'Corben', cursive;

  // display: block;
  background-color: #7591ae;
  // color: rgb(180, 35, 35);
  
}



.nav-item{
  font-family: 'Mukta', sans-serif;
  color: white;
  font-size: larger;
  font-weight: bold;
  padding: 7px;

}

// .nav-text{
//   font-family: 'Mukta', sans-serif;
//   color: white;
//   font-size: larger;
//   font-weight: bold;
// }
// .b-nav-item{
//     color: rgb(180, 35, 35);

// }

#hello{
  font-family: 'Mukta', sans-serif;
  color: white;
  font-size: larger;
  font-weight: 500;
  padding: 7px;
  padding-top: 14px;


}

#nav a{
  font-weight:500;
  color:white;
  padding: 7px;

}



#main {
  font-family: 'Corben', cursive;
  text-shadow: 2px 4px #000000;
  -webkit-text-stroke: 1px black;
  color: #fef5a2;
  // font-stretch: condensed;
  
}

#modal-link{
  padding: 7px;
}

// .b-dropdown-item{
//   color:#000000;
// }

// #nav a.router-link-exact-active {
//   color: #42b983;
// }
</style>
