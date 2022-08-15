<template>
  <nav class="navbar navbar-expand-lg text-center">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/"
        ><img class="logo" src="../assets/logo.png" alt=""
      /></router-link>
      <div
        class="bi bi-list navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      ></div>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link active" aria-current="page" to="/"
              >Home</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" :to="{ name: 'About' }"
              >About</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" :to="{ name: 'Perfumes' }"
              >Perfumes</router-link
            >
          </li>
          <!-- <li class="nav-item">
          <router-link class="nav-link"  :to="{name: 'Shop'}">Shop</router-link>
        </li>
        <li class="nav-item">
           <router-link class="nav-link"  :to="{name: 'Support'}">Support</router-link>
        </li>
        <li class="nav-item">
           <router-link class="nav-link"  :to="{name: 'Contact'}">ContactUs</router-link>
        </li>
        <li class="nav-item" v-if="isadmin== true">
           <router-link class="nav-link"  :to="{name: 'Admin'}">Dashboard</router-link>
        </li> -->
        </ul>
        <ul class="navbar-nav dr mb-2 mb-lg-0">
          <li class="nav-item" v-if="this.loggedin == false">
            <div
              class="nav-link"
              data-bs-toggle="modal"
              data-bs-target="#PerfumeModal"
            >
              Login
            </div>
          </li>
          <li class="nav-item dropdown">
            <a
              class="nav-link dropdown-toggle"
              href="#"
              id="navbarDropdown"
              role="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              <i class="bi bi-person-circle"></i>
            </a>
            <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
              <li>
                <router-link class="dropdown-item" :to="{ name: 'Profile' }"
                  >Profile</router-link
                >
              </li>
              <li v-if="this.loggedin == true">
                <div @click="logout" class="dropdown-item">Logout</div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <!-- Modal -->
  <div
    class="modal fade"
    id="PerfumeModal"
    tabindex="-1"
    aria-labelledby="mullinsModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-body">
          <Login />
        </div>
      </div>
    </div>
  </div>

  <div
    class="modal fade"
    id="registerModal"
    tabindex="-1"
    aria-labelledby="registerModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-body">
          <Register />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Login from "../views/Login.vue";
import Register from "../views/Registration.vue";
export default {
  data() {
    return {
      loggedin: false,
      isadmin: false,
    };
  },
  components: {
    Login,
    Register,
  },
  methods: {
    logout() {
      if (!localStorage.getItem("jwt")) {
        alert("No User To log Out");
        return this.$router.push({ name: "Home" });
      } else {
        localStorage.clear();
        this.loggedin = false;
        alert("user logged out");
        this.$router.push({ name: "Home" });
        location.reload();
      }
    },
  },
  mounted() {
    if (localStorage.getItem("jwt")) {
      fetch("https://mullins-marine-api.herokuapp.com/users/oneuser/", {
        method: "GET",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.loggedin = true;
          this.isadmin = json.isadmin;
        })
        .catch((err) => {
          alert(err);
        });
    }
  },
};
</script>

<style scoped>
.dropdown-item,
.nav-link {
  cursor: pointer;
}
.dr {
  margin-right: auto;
  margin-left: auto;
}
.navbar {
  background-color: #003459;
  position: sticky;
  top: 0;
  z-index: 1000;
}
.navbar-toggler {
  color: white;
  font-size: 2rem;
}
.logo {
  height: 50px;
}
.nav-link {
  color: white;
}
</style>
