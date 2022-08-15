<template>
  <section class="Home">
    
    
    <div class="container">
      <div class="row">
        <div class="col">
          <h1>Smell good</h1>
          <p>
            Smell good
          </p>
          <router-link to="Encyclopedia"
            ><button class="button">Perfumes</button></router-link
          >
          <router-link to="Shop"
            ><button class="button">Shop</button></router-link
          >
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Login from "../views/Login.vue";
export default {
  name: "Home",
  components: {
    Login,
  },
  mounted() {
    $(".Home").ripples({
  resolution: 200,
  perturbance: 0.04,
});
  },
};
</script>

<style scoped>
.button {
  margin-left: 30px;
  text-decoration: none;
  background-color: #003459;
  color: white;
  border-radius: 300px;
  border: none;
  width: 120px;
  height: 40px;
}
.button:hover {
  transform: scale(1.1);
}
.container {
  color: white;
  position: absolute;
  text-align: center;
  top: 42%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%);
  z-index: 1;
}
.Home {
padding: 0;
  margin: 0;
  width: 100%;
  height: 100vh;
  position: fixed;
}
#myVideo {
  z-index: -1000;
  position: absolute;
  right: 0;
  bottom: 0;
  min-width: 100%;
  min-height: 100%;
}
</style>