<template>
  <div class="app">
    <header>
      <nav class="navbar">
        <div class="navbar-brand">
          <h1>{{ sitename }}</h1>
        </div>
        <div class="navbar-menu">
          <button
            class="fas fa-shopping-cart"
            @click="toggleCart"
            :disabled="cartItemCount === 0"
          >
            {{ this.cart.length }}
          </button>
        </div>
      </nav>
    </header>
    <main>
      <product-list
        :products="products"
        @addProduct="addToCart"
        v-if="showProduct"
      ></product-list>
      <checkout :cart="cart" @remove-item="removeProduct" v-else></checkout>
    </main>
  </div>
</template>

<script>
/* eslint-disable */
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
    addToCart(product) {
      console.log("addProduct event received by the root component.");
      product.availableInventory--;
      this.cart.push(product);
      
    },


    toggleCart() {
      this.showProduct = !this.showProduct;
    },

    removeProduct(id) {
      const index = this.cart.findIndex((product) => product.id === id);

      // If the product is found, remove it from the cart
      if (index !== -1) {
        this.cart.splice(index, 1);
      }
    },
  },

  computed: {
    cartItemCount: function () {
      return this.cart.length;
    },
  },
};
</script>


