<template>
  <div id="about" class="container">
    <h1 id="about-title">About</h1>
    <br />
    <div id="about-content">
      <strong>
        Created by Ariel Perelman & Diana Prochorov - 3rd year ISE students.<br />
        This is a Full-stack web API. Requests are sent by client side
        application and processed by the server.<br />
        The backend is implemented using Node JS and Express.JS, and a MySQL
        database. The frontend is implemented using VUE.JS.<br />
        Here are some other projects we did:<br />
        <div id="links">
          <a
            href="https://github.com/Web-Development-Environments-2022/assignment2-321747388_205628217"
            >PACMAN game</a
          ><br />
          <a
            href="https://github.com/Web-Development-Environments-2022/205628217"
            >Ariel's website for the elderly</a
          ><br />
          <a
            href="https://github.com/Web-Development-Environments-2022/321747388"
            >Diana's website for the elderly</a
          >
        </div>
      </strong>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style>
#about {
  margin-top: 70px;
  font-size: 23px;
  color: rgb(6, 6, 6);
  max-width: 800px;
  border-color: rgba(5, 5, 5, 0.849);
  border-width: 1px;
  background-color: bisque;
  border-style: solid;
  border-radius: 5px;
}

#about-title {
  font-family: "Corben", cursive;
  text-shadow: 2px 3.5px #000000;
  -webkit-text-stroke: 1.2px black;
  color: #ebc2ce;
  font-size: 50px;
  text-align: center;
}

a {
  color: steelblue;
}

#links {
  text-align: center;
}
</style>
