<template>
  <form
    class="container rounded bg-white mb-5 text-center"
    @submit.prevent="modUser"
    v-if="users"
  >
    <div class="row">
      <div class="col-md-12 border-right">
        <div class="py-5">
          <div class="d-flex justify-content-between align-items-center mb-3">
            <h4 class="text-right">Profile Settings</h4>
          </div>
        </div>
      </div>
      <div class="row mt-2">
        <div class="col-md-12">
          <label class="labels">Name</label
          ><input
            type="text"
            class="form-control"
            :placeholder="users.fullname"
            v-model="name"
          />
        </div>
        <div class="col-md-12">
          <label class="labels">Password</label
          ><input
            type="password"
            class="form-control"
            placeholder="Password"
            v-model="password"
          />
        </div>
      </div>
      <div class="row mt-3">
        <div class="col-md-12">
          <label class="labels">Mobile Number</label
          ><input
            type="number"
            class="form-control"
            :placeholder="users.phone_number"
            v-model="phone_number"
          />
        </div>
        <div class="col-md-12">
          <label class="labels">Street Address</label
          ><input
            type="text"
            class="form-control"
            :placeholder="users.street"
            v-model="street"
          />
        </div>
        <div class="col-md-12">
          <label class="labels">Zipcode</label
          ><input
            type="text"
            class="form-control"
            :placeholder="users.zipcode"
            v-model="zipcode"
          />
        </div>
        <div class="col-md-12">
          <label class="labels">Email</label
          ><input
            type="text"
            class="form-control"
            :placeholder="users.email"
            v-model="email"
          />
        </div>
      </div>
      <div class="row mt-3">
        <div class="col-md-6">
          <label class="labels">Country</label
          ><input
            type="text"
            class="form-control"
            :placeholder="users.country"
            v-model="country"
          />
        </div>
        <div class="col-md-6">
          <label class="labels">City/Region</label
          ><input
            type="text"
            class="form-control"
            v-model="city"
            :placeholder="users.city"
          />
        </div>
      </div>
      <div class="mt-5 text-center">
        <button
          class="btn btn-primary profile-button"
          data-bs-dismiss="modal"
          type="submit"
        >
          Save Profile
        </button>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      users: null,
      name: "",
      email: "",
      phone_number: "",
      password: "",
      street: "",
      zipcode: "",
      country: "",
      city: "",
      renderComponent: true,
    };
  },
  methods: {
    modUser() {
      if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Login" });
      }
      fetch("", {
        method: "PUT",
        body: JSON.stringify({
          fullname: this.name,
          email: this.email,
          phone_number: this.phone_number,
          password: this.password,
          street: this.street,
          zipcode: this.zipcode,
          country: this.country,
          city: this.city,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("User Updated");
          this.$router.go();
        })
        .catch((err) => {
          alert(err);
        });
    },
  },
  mounted() {
    if (!localStorage.getItem("jwt")) {
      alert("User not logged in");
      return this.$router.push({ name: "Home" });
    }
    fetch("", {
      method: "GET",
      headers: {
        "Content-type": "application/json; charset=UTF-8",
        Authorization: `Bearer ${localStorage.getItem("jwt")}`,
      },
    })
      .then((response) => response.json())
      .then((json) => {
        this.users = json;
      })
      .catch((err) => {
        alert(err);
      });
  },
};
</script>

<style scoped>
.form-control:focus {
  box-shadow: none;
  border-color: #222222;
}
.profile-button {
  background: #003459;
  box-shadow: none;
  border: none;
}
.profile-button:hover {
  background: #222222;
}
.profile-button:focus {
  background: #000000;
  box-shadow: none;
}
.profile-button:active {
  background: #000000;
  box-shadow: none;
}
.back:hover {
  color: #000000;
  cursor: pointer;
}
.labels {
  font-size: 11px;
}
.add-experience:hover {
  background: #222222;
  color: #fff;
  cursor: pointer;
  border: solid 1px #111111;
}
</style>