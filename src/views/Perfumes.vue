<template>
  <section id="Perfumes">
    <div class="container">
      <div class="row toprow">
        <div class="col-12 topcol">
          <h1>PERFUMES</h1>
        </div>
        <div class="col-3"></div>
        <div class="col-12 col-sm-12 col-md-6 col-lg-6 col-xl-6">
          <p class="para">
            Explore the Marine Life Encyclopedia to learn fun facts and more
            about your favorite marine animals. From sharks and sea turtles to
            ecosystems and corals, you’re in the right place to take a deep dive
            into life under the sea.
          </p>
        </div>
        <div class="col-3"></div>
      </div>
      <div class="row">
        <router-link to="SummerFemales" class="col-12 col-sm-12 col-md-12 col-lg-6 SummerFemales enchover">
          <h2> Female Summer Perfumes</h2>
        </router-link>
        <router-link to="SummerMales" class="col-12 col-sm-12 col-lg-6 SummerMales enchover">
          <h2>Males Summer Perfumes</h2>
        </router-link>
      </div>
      <div class="row">
        <router-link to="WinterFemales" class="col-12 col-sm-12 col-lg-6 WinterFemales enchover">
          <h2>Females Winter Perfumes</h2>
        </router-link>
        <router-link to="WinterMales" class="col-12 col-sm-12 col-lg-6 WinterMales enchover">
          <h2>Winter Males Perfumes</h2>
        </router-link>
      </div>
      </div>
    
  </section>
</template>

<script>
export default {};
</script>

<style scoped>
.SummerFemales {
  background: linear-gradient(0deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)),url("");
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 500px;
  background-position: center;
}
.SummerMales {
  background: linear-gradient(0deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)),
    url("");
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 500px;
  background-position: center;
}
.WinterFemales {
  background: linear-gradient(0deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)),
    url("");
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 500px;
  background-position: center;
}
.WinterMales {
  background: linear-gradient(0deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)),
    url("");
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 500px;
  background-position: center;
}

.SummerFemales,
.SummerMales,
.WinterFemales,
.WinterMales{
  text-decoration: none;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.enchover:hover {
  cursor: pointer;
  filter: blur(1px) brightness(90%);
  transition: all 250ms ease-in-out;
}
@media (min-width: 1200px) {
  .container {
    max-width: 100%;
  }
  .topcol {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
}
</style>