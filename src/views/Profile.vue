<template>
  <section>
    <div class="container rounded bg-white mt-5 mb-5 text-center" v-if="users">
      <div class="row">
        <div class="col-md-3 border-right">
          <div
            class="d-flex flex-column align-items-center text-center p-3 py-5"
          >
            <img
              class="rounded-circle mt-5"
              src="https://i.postimg.cc/W375kJP0/petals.jpg"
            /><span class="font-weight-bold">{{ users.fullname }}</span
            ><span class="text-black-50">{{ users.email }}</span
            ><span> </span>
          </div>
        </div>
        <div class="col-md-5 border-right">
          <div class="p-3 py-5">
            <div class="d-flex justify-content-between align-items-center mb-3">
              <h4 class="text-right">Profile & Shipping Address</h4>
            </div>
            <div class="row mt-2">
              <table class="table table-user-information">
                <tbody>
                  <tr>
                    <td>Name:</td>
                    <td>{{ users.fullname }}</td>
                  </tr>
                  <tr>
                    <td>Email:</td>
                    <td>{{ users.email }}</td>
                  </tr>
                  <tr>
                    <td>Shipping Address:</td>
                    <td>
                      {{ users.street }}<br /><br />
                      {{ users.city }}<br /><br />
                      {{ users.zipcode }}<br /><br />
                      {{ users.country }}
                    </td>
                  </tr>
                  <tr>
                    <td>Phone:</td>
                    <td>{{ users.phone_number }}</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <div class="mt-5 text-center">
              <button
                data-bs-toggle="modal"
                data-bs-target="#editUser"
                class="btn btn-primary profilebtn"
                type="button"
              >
                Edit
              </button>
            </div>
            <div v-if="loading">
              <div class="half-circle-spinner">
                <div class="circle circle-1"></div>
                <div class="circle circle-2"></div>
              </div>
            </div>
            <div class="mt-5 text-center">
              <button
                class="btn btn-danger profile-button"
                type="button"
                @click="deleteUser"
              >
                Destroy User
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="else" v-else>
      <div class="boxes">
        <div class="box">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
        <div class="box">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
        <div class="box">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
        <div class="box">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>
    </div>
    <!-- Modal -->
    <div
      class="modal fade"
      id="editUser"
      tabindex="-1"
      aria-labelledby="editUserLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-body">
            <ProfileEdit />
          </div>
        </div>
      </div>
    </div>
    <!-- Modal -->
  </section>
</template>

<script>
import ProfileEdit from "../views/ProfileEdit.vue";
export default {
  components: {
    ProfileEdit,
  },
  data() {
    return {
      users: null,
      email: null,
      loading: false,
    };
  },
  methods: {
    async deleteUser() {
      if (confirm("Do you really want to delete your profile?")) {
        if (!localStorage.getItem("jwt")) {
          alert("User not logged in");
          return this.$router.push({ name: "Home" });
        }
        this.loading = true;
        try {
          fetch("http://localohost:6969/contact/DeleteUser", {
            method: "POST",
            body: JSON.stringify({
              email: this.email,
            }),
            headers: {
              "Content-type": "application/json; charset=UTF-8",
            },
          })
            .then((response) => response.json())
            .then((json) => {
              this.loading = false;
              fetch("http://localohost:6969/users", {
                method: "DELETE",
                headers: {
                  "Content-type": "application/json; charset=UTF-8",
                  Authorization: `Bearer ${localStorage.getItem("jwt")}`,
                },
              })
                .then((response) => response.json())
                .then((json) => {
                  alert("DELETED USER");
                  localStorage.clear();
                  return this.$router.push({ name: "Home" });
                })
                .catch((err) => {
                  alert(err);
                });
            });
        } catch (error) {
          alert(err);
          this.loading = false;
        }
      }
    },
  },
  mounted() {
    if (!localStorage.getItem("jwt")) {
      return this.$router.push({ name: "Home" });
    }
    fetch("http://localohost:6969/users/oneuser/", {
      method: "GET",
      headers: {
        "Content-type": "application/json; charset=UTF-8",
        Authorization: `Bearer ${localStorage.getItem("jwt")}`,
      },
    })
      .then((response) => response.json())
      .then((json) => {
        this.users = json;
        this.email = json.email;
      })
      .catch((err) => {
        alert(err);
      });
  },
};
</script>

<style scoped>
.half-circle-spinner,
.half-circle-spinner * {
  box-sizing: border-box;
}
.half-circle-spinner {
  z-index: 2627;
  width: 60px;
  height: 60px;
  border-radius: 100%;
  position: fixed;
  top: 50%;
  left: 50%;
}
.half-circle-spinner .circle {
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 100%;
  border: calc(60px / 10) solid transparent;
}
.half-circle-spinner .circle.circle-1 {
  border-top-color: white;
  animation: half-circle-spinner-animation 1s infinite;
}
.half-circle-spinner .circle.circle-2 {
  border-bottom-color: #1d92ff;
  animation: half-circle-spinner-animation 1s infinite alternate;
}
@keyframes half-circle-spinner-animation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.else {
  min-height: 100vh;
  font-family: Roboto, Arial;
  color: #adafb6;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(249, 251, 255, 0.6);
}
.boxes {
  height: 32px;
  width: 32px;
  position: relative;
  -webkit-transform-style: preserve-3d;
  transform-style: preserve-3d;
  -webkit-transform-origin: 50% 50%;
  transform-origin: 50% 50%;
  margin-top: 32px;
  -webkit-transform: rotateX(60deg) rotateZ(45deg) rotateY(0deg) translateZ(0px);
  transform: rotateX(60deg) rotateZ(45deg) rotateY(0deg) translateZ(0px);
}
.boxes .box {
  width: 32px;
  height: 32px;
  top: 0px;
  left: 0;
  position: absolute;
  -webkit-transform-style: preserve-3d;
  transform-style: preserve-3d;
}
.boxes .box:nth-child(1) {
  -webkit-transform: translate(100%, 0);
  transform: translate(100%, 0);
  -webkit-animation: box1 1s linear infinite;
  animation: box1 1s linear infinite;
}
.boxes .box:nth-child(2) {
  -webkit-transform: translate(0, 100%);
  transform: translate(0, 100%);
  -webkit-animation: box2 1s linear infinite;
  animation: box2 1s linear infinite;
}
.boxes .box:nth-child(3) {
  -webkit-transform: translate(100%, 100%);
  transform: translate(100%, 100%);
  -webkit-animation: box3 1s linear infinite;
  animation: box3 1s linear infinite;
}
.boxes .box:nth-child(4) {
  -webkit-transform: translate(200%, 0);
  transform: translate(200%, 0);
  -webkit-animation: box4 1s linear infinite;
  animation: box4 1s linear infinite;
}
.boxes .box > div {
  background: #5c8df6;
  --translateZ: 15.5px;
  --rotateY: 0deg;
  --rotateX: 0deg;
  position: absolute;
  width: 100%;
  height: 100%;
  background: #5c8df6;
  top: auto;
  right: auto;
  bottom: auto;
  left: auto;
  -webkit-transform: rotateY(var(--rotateY)) rotateX(var(--rotateX))
    translateZ(var(--translateZ));
  transform: rotateY(var(--rotateY)) rotateX(var(--rotateX))
    translateZ(var(--translateZ));
}
.boxes .box > div:nth-child(1) {
  top: 0;
  left: 0;
  background: #5c8df6;
}
.boxes .box > div:nth-child(2) {
  background: #145af2;
  right: 0;
  --rotateY: 90deg;
}
.boxes .box > div:nth-child(3) {
  background: #447cf5;
  --rotateX: -90deg;
}
.boxes .box > div:nth-child(4) {
  background: #dbe3f4;
  top: 0;
  left: 0;
  --translateZ: -90px;
}
@keyframes box1 {
  0%,
  50% {
    transform: translate(100%, 0);
  }
  100% {
    transform: translate(200%, 0);
  }
}
@keyframes box2 {
  0% {
    transform: translate(0, 100%);
  }
  50% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(100%, 0);
  }
}
@keyframes box3 {
  0%,
  50% {
    transform: translate(100%, 100%);
  }
  100% {
    transform: translate(0, 100%);
  }
}
@keyframes box4 {
  0% {
    transform: translate(200%, 0);
  }
  50% {
    transform: translate(200%, 100%);
  }
  100% {
    transform: translate(100%, 100%);
  }
}
@-webkit-keyframes animation-bg {
  0% {
    background-position: 50% 100%;
  }
  100% {
    background-position: 50% 0%;
  }
}
@keyframes animation-bg {
  0% {
    background-position: 50% 100%;
  }
  100% {
    background-position: 50% 0%;
  }
}
.form-control:focus {
  box-shadow: none;
  border-color: #222222;
}
.profilebtn {
  background: rgb(0, 0, 0);
  box-shadow: none;
  border: none;
}
.profile-button:hover {
  background: #222222;
}
.profile-button:active {
  background: red;
  box-shadow: none;
}
.back:hover {
  color: #003459;
  cursor: pointer;
}
.labels {
  font-size: 11px;
}
.rounded-circle {
  height: 280px;
}
.add-experience:hover {
  background: #222222;
  color: #fff;
  cursor: pointer;
  border: solid 1px #111111;
}
</style>