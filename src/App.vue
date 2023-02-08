<template>
  <div id="app">
    <nav class="navbar bg-primary" data-bs-theme="dark">
      <div class="d-flex sidenav">
        <div>
          SORT the Data
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="sortBy"
              v-model="selectedText"
              checked
              id="subject"
              :value="'name'"
            />
            <label class="form-check-label" for="subject">
              Subject Name
            </label>
          </div>
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="sortBy"
              v-model="selectedText"
              id="location"
              :value="'location'"
            />
            <label class="form-check-label" for="location">
              Location
            </label>
          </div>
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="sortBy"
              v-model="selectedText"
              id="price"
              :value="'price'"
            />
            <label class="form-check-label" for="price">
              Price
            </label>
          </div>
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="sortBy"
              v-model="selectedText"
              id="availability"
              :value="'spaces'"
            />
            <label class="form-check-label" for="availability">
              Number of Availability 
            </label>
          </div>
        </div>
        <div class="mt-3">
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="ascDec"
              v-model="ascDec"
              id="asc"
              :value="1"
            />
            <label class="form-check-label" for="asc">
              Ascending Order
            </label>
          </div>
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="ascDec"
              v-model="ascDec"
              id="desc"
              :value="2"
            />
            <label class="form-check-label" for="desc">
              Descending Order
            </label>
          </div>
        </div>
      </div>
    </nav>
    <div id="app" class="flex-container">
     
      <div class="content">
        <h1>Shopping Cart</h1>
        <h6 class="text-danger" v-if="showMsg">
          Please Enter your Name and Phone number
        </h6>
        <div class="container mx-auto mt-4">
          <div class="form-group d-flex">
            <input
              type="email"
              class="form-control w-50 "
              id="search"
              placeholder="Search for Subject"
              v-model="searchTEXT"
            />
            <button class="btn btn-danger mx-1" @click="searchByText()">
              Search
            </button>
          </div>

          <div class="d-flex flex-wrap w-100 justify-content-between">
            <div class=" me-1 card" v-for="(product, index) in LISTOFLESSONS">
              <div class="card-body card-h">
                <!-- <h5 class="card-title">{{ product.name }}</h5> -->
                <img :src="imageSRC" class="card-img-top" :alt="imageSRC" />
                <div
                  class="mt-2 card-body-content d-flex flex-column justify-content-between"
                >
                  <div class="mt-2 ">
                    <h6 class="card-subtitle mb-2 text-muted">
                      Name: {{ product.name }}
                    </h6>
                    <h6 class="card-subtitle mb-2 text-muted">
                      Location: {{ product.location }}
                    </h6>
                    <h6 class="card-subtitle mb-2 text-muted">
                      Price: {{ product.price }}
                    </h6>
                    <h6 class="card-subtitle mb-2 text-muted">
                      Spaces : {{ product.space }}
                    </h6>
                  </div>
                  <div>
                    <button
                      class="btn btn-success"
                      :disabled="isBtn"
                      @click="BUYlessons(product, index)"
                    >
                      Add to cart
                    </button>
                    <button
                      class="btn btn-danger"
                      @click="DELETETHELESSONS(product, index)"
                    >
                      Delete
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <h3 class="mt-3" v-if="LESSONSTOSELECTED.length">Shopping Cart</h3>
        <div
          class="d-flex flex-wrap w-100 justify-content-between"
          v-if="LESSONSTOSELECTED.length"
        >
          <div class=" me-1 card" v-for="(product, index) in LESSONSTOSELECTED">
            <div class="card-body">
              <img :src="product.img" class="card-img-top" alt="..." />
              <div class="mt-2 d-flex flex-column justify-content-between">
                <div class="mt-2 ">
                  <h6 class="card-subtitle mb-2 text-muted">
                    UserName: {{ product.name }}
                  </h6>
                  <h6 class="card-subtitle mb-2 text-muted">
                    lesson Name: {{ product.lessonName }}
                  </h6>
                  <h6 class="card-subtitle mb-2 text-muted">
                    Phone Number: {{ product.phoneNumber }}
                  </h6>
                  <h6 class="card-subtitle mb-2 text-muted">
                    No. of Spaces : {{ product.spaces }}
                  </h6>
                </div>
                <div>
                  <button
                    class="btn btn-primary  "
                    :disabled="isBtn"
                    @click="removeFromCart(product, index)"
                  >
                    Remove from cart
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <h3>Checkout</h3>
        <form class="mt-3">
          Name:
          <input
            type="text"
            v-model="form.name"
            class="form-group"
            placeholder="Name"
          />
          Phone:
          <input
            type="number"
            v-model="form.number"
            class="form-group"
            placeholder="Phone"
          />
          <button class="btn btn-primary">Checkout</button>
        </form>
      </div>
    </div>
    
  </div>
</template>

<script>
// import { ProductList } from "./lessons";

export default {
  name: "app",
  data() {
    return {
      searchTEXT: "",
      numberofavalaibleProducts: 5,
      isBtn: false,
      showMsg: false,
      LISTOFLESSONS: [],
      LESSONSTOSELECTED: [],
      ascDEcending: "asc",
      ascDecs: 1,
      sortSelected: "name",
      searchValue: "",
      form: {
        name: "",
        number: ""
      },
      imageSRC: ""
    };
  },
  computed: {
    ascDec: {
      get() {
        return this.ascDecs;
      },
      set(newValue) {
        this.ascDEcending = newValue == 1 ? "asc" : "dsc";
        this.SORTTHELESSON();
      }
    },
    selectedText: {
      get() {
        return this.sortSelected;
      },
      set(newValue) {
        this.sortSelected = newValue;
        this.sortList();
      }
    }
  },
  created() {
    this.GETLESSONSLISTS();
    this.ImageLOADING();
    this.SORTTHELESSON();
  },
  methods: {
    ImageLOADING() {
      fetch("http://localhost:3000/lesson-images", {
        method: "GET"
      })
        .then(res => {
          let data = res.json();
          return data;
        })
        .then(res => {
          this.imageSRC = res;
          console.log(this.imageSRC);
        })
        .catch(responsesss => {
          
        });
    },
    searchByText() {
      if (this.searchTEXT.length) {
        fetch(`http://localhost:3000/search`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ text: this.searchTEXT })
        })
          .then(res => {
            let data = res.json();
            return data;
          })
          .then(res => {
            this.LISTOFLESSONS = [];
            this.LISTOFLESSONS = res;
            console.log(res);
          })
          .catch(responsesss => {
            console.log("erosss", responsesss);
          });
      } else {
        this.GETLESSONSLISTS();
      }
    },
    deleteAllOrders() {
      fetch(`http://localhost:3000/orders`, {
        method: "DELETE",
        headers: {
          "Access-Control-Allow-Headers": "Content-Type"
        }
      })
        .then(res => {
          console.log("deleted successfully");
          this.GETLESSONSLISTS();
          return res;
        })
        .catch(responsesss => {
          console.log("s", responsesss);
        });
    },
    DELETETHELESSONS(product, indexxx) {
      fetch(`http://localhost:3000/lessons/${product._id}`, {
        method: "DELETE",
        headers: {
          "Access-Control-Allow-Headers": "Content-Type"
        }
      })
        .then(res => {
          console.log("deleted successfully");
          this.GETLESSONSLISTS();
          return res;
        })
        .catch(responsesss => {
          console.log("s", responsesss);
        });
    },
    GETLESSONSLISTS() {
      fetch("http://localhost:3000/lessons", {
        method: "GET"
      })
        .then(res => {
          let data = res.json();
          return data;
        })
        .then(res => {
          this.LISTOFLESSONS = [];
          this.LISTOFLESSONS = res;
          console.log(res);
        })
        .catch(responsesss => {
          console.log("error", responsesss);
        });
    },
    GETSelectedOrders() {
      fetch("http://localhost:3000/orders", {
        method: "GET"
      })
        .then(res => {
          let data = res.json();
          return data;
        })
        .then(res => {
          this.LESSONSTOSELECTED = [];
          this.LESSONSTOSELECTED = res;
          console.log(res);
        })
        .catch(responsesss => {
          console.log("error", responsesss);
        });
    },
    SORTTHELESSON() {
      if (this.ascDEcending == "asc") {
        this.sortList();
      } else {
        this.LISTOFLESSONS = this.LISTOFLESSONS.reverse();
      }
    },
    sortList() {
      this.LISTOFLESSONS.sort((a, b) => {
        let fa =
          typeof a == "string"
            ? a[this.sortSelected].toLowerCase()
            : a[this.sortSelected];
        let fb =
          typeof a == "string"
            ? b[this.sortSelected].toLowerCase()
            : b[this.sortSelected];
        if (fa < fb) {
          return -1;
        }
        if (fa > fb) {
          return 1;
        }
        return 0;
      });
      // this.SORTTHELESSON();
    },
    BUYlessons(product, index) {
      console.log(this.form, product);
      if (this.form.name && this.form.number) {
        this.showMsg = false;
        let order = {
          name: this.form.name,
          phoneNumber: this.form.number,
          lessonId: product._id,
          spaces: product.space,
          lessonName: product.name
        };
        fetch("http://localhost:3000/orders", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(order)
        })
          .then(res => {
            console.log(res);
            this.GETLESSONSLISTS();
            this.GETSelectedOrders();
          })
          .catch(error => {
            console.log(error);
          });
      } else {
        this.showMsg = true;
      }
    },
    removeFromCart(product, cart) {}
  }
};
</script>

<style>
html,
body {
  margin: 0 !important;
  padding: 0 !important;
  width: 100%;
}



.flex-container {
  display: -webkit-flex;
  display: flex;
  background-color: rgb(255, 238, 47);
  min-height: 100%;
}

.sidenav {
  background-color: #e1f868;
  -webkit-flex: 1;
  flex: 1;
}

.content {
  background-color: rgb(248, 253, 255);
  padding: 10px;
  -webkit-flex: 5;
  flex: 5;
  height: 2000px;
}

.form-check {
  justify-content: space-between;
  display: flex;
  padding-left: 30px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

.card {
  max-height: 380px;
  height: 380px;
  width: 200px;
  margin: 5px 5px;
}

.card-body-content {
  height: 180px;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.card-body {
  padding: 0.5rem;
}

h1,
h2 {
  font-weight: normal;
}



a {
  color: #42b983;
}
</style>
