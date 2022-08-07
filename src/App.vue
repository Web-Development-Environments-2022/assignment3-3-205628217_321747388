<template>
  <div id="app">
    <div>
      <b-navbar toggleable="md" type="dark" variant="info" sticky>
        <b-navbar-brand :to="{ name: 'main' }">Main</b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
            <b-nav-item :to="{ name: 'about' }">About</b-nav-item>
          </b-navbar-nav>

          <b-navbar-nav class="ml-auto" v-if="!$root.store.username">
            <b-nav-text>Hello Guest!</b-nav-text>
            <b-nav-item :to="{ name: 'register' }">Register</b-nav-item>
            <b-nav-item :to="{ name: 'login' }">Login</b-nav-item>
          </b-navbar-nav>
          <b-navbar-nav class="ml-auto" v-else>
            <b-nav-text>Hello {{ $root.store.username }}!</b-nav-text>
            <b-nav-item :to="{ name: 'addRecipe' }">Add Recipe</b-nav-item>
            <!-- <b-button v-b-modal.modal-1>Add Recipe</b-button> -->
            <b-nav-item-dropdown text="Personal Recipes" right>
              <b-dropdown-item :to="{ name: 'favorites' }">Favorites</b-dropdown-item>
              <b-dropdown-item :to="{ name: 'myRecipes' }">My Recipes</b-dropdown-item>
              <b-dropdown-item :to="{ name: 'familyRecipes' }">Family Recipes</b-dropdown-item>
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

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
