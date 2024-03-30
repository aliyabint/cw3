<template>
  <div class="app">
    <header>
      <nav class="navbar">
        <div class="navbar-brand">
          <h1>{{ sitename }}</h1>
        </div>
        <div class="navbar-menu">
          <button
            class="fas fa-cart-arrow-down"
            id="cicon"
            @click="showCheckout"
            :disabled="cartItemCount === 0"
            v-if="showProduct"
          >
            {{ this.cart.length }}
          </button>
          <button class="fas fa-cart-arrow-down" id="cicon" v-else>
            {{ this.cart.length }}
          </button>
        </div>
      </nav>
    </header>
    <main>
      <product-list :products="products" @addProduct="addToCart"></product-list>
      <checkout :cart="cart" @remove-item="removeProduct"></checkout>
    </main>
  </div>
</template>

<script>
import productList from "./components/ProductList.vue";
import checkout from "./components/Form.vue";

export default {
  name: "App",
  components: {
    productList,
    checkout,
  },
  data() {
    return {
      sitename: "your guide",
      showProduct: true,
      cart: [],
      products: [], // Ensure products is properly initialized
    };
  },

  created() {
    console.log("requesting data from server");
    // Fetch initial product data from server
    fetch("http://localhost:3000/collection/classes").then((response) => {
      response.json().then((json) => {
        this.products = json; // Assign fetched data to products
        console.log(json);
      });
    });
  },
  methods: {
    showCheckout() {},
    addToCart(product) {
      console.log("addProduct event received by the root component.");
      this.cart.push(product);
    },
  },

  removeProduct(id) {
    let found = false;
    function rearrangingCart(cartItem) {
      if (found == false) {
        if (cartItem == id) {
          found = true;
        } else {
          return cartItem;
        }
      } else {
        return cartItem;
      }
    }
    this.cart = this.cart.filter(rearrangingCart);
    var prodid = this.cart.findIndex((product) => product.id === id);
  },

  toggleCart() {
    this.showProduct = !this.showProduct;
  },

  computed: {
    cartItemCount: function () {
      return this.cart.length || " ";
    },
  },
};
</script>


