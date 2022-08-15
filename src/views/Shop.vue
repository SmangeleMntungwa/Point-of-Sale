<template>
  <section id="shop">
    <div class="container" v-if="product">
      <button
        @click="isModalVisible = true"
        type="button"
        class="cart"
        data-bs-toggle="modal"
        data-bs-target="#exampleModal"
      >
        Cart
      </button>
      <div class="row">
        <div class="col">
          <h1>Shop</h1>
        </div>
      </div>
      <div class="row">
        <div class="col-4">
          <select
            v-model="selected"
            class="form-select"
            aria-label="Default select example"
          >
            <option selected value="">Display All</option>
            <option value="Male Summer Perfumes">Male Summer Perfumes</option>
            <option value="wetsuits">Female Summer Perfumes</option>
            <option value="Male Winter Perfumes">Male Winter Perfumes</option>
            <option value="Female Winter Perfumes">Female Winter Perfumes</option>
          </select>
        </div>
        <div class="col-4">
          <form class="d-flex">
            <input
              class="form-control me-2"
              type="text"
              v-model="search"
              placeholder="Search"
              aria-label="Search"
            />
          </form>
        </div>
        <div class="col-4"></div>
      </div>
      <div class="row">
        <div
          class="col-lg-4 col-md-6 mb-3"
          v-for="products of filterProducts"
          :key="products._id"
        >
          <div class="card py-4 px-lg-5 h-100">
            <div class="card-body d-flex flex-column">
              <div class="text-center">
                <img
                  :src="products.img"
                  class="img-fluid mb-5"
                  alt="Websearch"
                />
              </div>

              <h2 class="card-title mb-4 text-center">{{ products.title }}</h2>
              <div class="pricing">
                <ul class="list-unstyled">
                  <li class="mb-3">
                    <span class="small ms-3">{{ products.description }}</span>
                  </li>
                </ul>
              </div>
              <div class="text-center mt-auto mb-4">
                <span class="fw-bold fs-2">R{{ products.price }}</span>
              </div>
              <div class="text-center">
                <button
                  type="button"
                  class="btn btncolor"
                  @click="addToCart(products._id)"
                >
                  Add to cart
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
   
    <div
      v-if="isModalVisible"
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <Cart />
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            

            <button
              class="btn btn-secondary"
              data-bs-dismiss="modal"
              @click="clearCart"
            >
              Clear Cart
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Cart from "../components/Cart.vue";
export default {
  components: {
    Cart,
  },
  data() {
    return {
      product: null,
      search: "",
      isModalVisible: false,
  
      selected: "",
    };
  },
  methods: {
    checkout(){
      
    },
    clearCart() {
      fetch("http://localohost:6969/", {
        method: "DELETE",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          location.reload();
        })
        .catch((err) => {
          alert(err);
        });
    },
    addToCart(id) {
      if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Shop" });
      } else {
        let cart = 1;
        fetch(`http://localohost:6969/${id}/`, {
          method: "POST",
          body: JSON.stringify({
            quantity: cart,
          }),
          headers: {
            "Content-type": "application/json; charset=UTF-8",
            Authorization: `Bearer ${localStorage.getItem("jwt")}`,
          },
        })
          .then((response) => response.json())
          .then((json) => {
            alert("Added to Cart");
            location.reload();
          })
          .catch((err) => {
            alert(err);
          });
      }
    },
   
  },
  mounted() {
    fetch("http://localohost:6969/product", {
      method: "GET",
      headers: {
        "Content-type": "application/json; charset=UTF-8",
      },
    })
      .then((response) => response.json())
      .then((json) => {
        this.product = json;
      })
      .catch((err) => {
        alert(err);
        console.log(err);
      });
  },
  computed: {
    filterProducts: function () {
      let filtered = this.product
      if (this.selected == '') {
          filtered = filtered.filter((product) => {
           return product.category.match(this.selected) ;
          
        });
        if(this.search){
          filtered = filtered.filter((product) =>{
            return product.title.match(this.search)
          })
        }
        return filtered
      }
      if (this.selected) {
        filtered = filtered.filter((product) => {
           return product.category.match(this.selected) ;
          
        });
        if(this.search){
          filtered = filtered.filter((product) =>{
            return product.title.match(this.search)
          })
        }
        return filtered
        
      }
  
      
    },
  },
};
</script>

<style scoped>
.else {
  min-height: 100vh;
  font-family: Roboto, Arial;
  color: #adafb6;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(249, 251, 255, 0.6);
}
.loading-text{
  color: black;
}


 @keyframes appear {
	 0% {
		 opacity: 0;
	}
	 95% {
		 opacity: 0;
	}
	 96% {
		 opacity: 1;
	}
}
 
.cart {
  background-color: #003459;
  float: right;
  color: white;
  border-radius: 300px;
  border: none;
  width: 100px;
  height: 40px;
}
.cart:hover {
  transform: scale(1.1);
  color: white;
}
.container {
  text-align: center;
}
.img-fluid {
  height: 250px;
}
.btncolor {
  background-color: #003459;
  border-radius: 55555px;
  padding: 1rem 2rem !important;
  color: #ffff;
  margin-bottom: 2rem;
}
.btn-danger,
.btn-warning {
  border-radius: 55555px;
  padding: 1rem 2rem !important;
  color: #ffff;
  margin-bottom: 2rem;
}
.package {
  list-style-type: none;
}
.parent {
  height: 300px;
  display: flex;
  border: 2px solid yellow;
}
.child {
  background-color: red;
  margin-bottom: auto;
  margin-left: auto;
  margin-right: auto;
  color: white;
}
.icon-color {
  color: #1f38fa;
}
</style>