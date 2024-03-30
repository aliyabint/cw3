<template>
  <div>
    <div
      class="products"
      style="
        width: 80%;
        height: 100%;
        background-color: #f6fff8;
        overflow: auto;
        padding: 2%;
      "
    >
      <div
        class="productblock"
        style="padding-top: 10px padding-left: 10px; display: flex; flex-wrap: wrap;"
      >
        <div
          class="productDetails"
          v-for="product in products"
          :key="product.id"
          style="
            margin: 2%;
            background-color: #cce3de;
            padding: 5%;
            border-radius: 15%;
            width: 30%;
            font-family: Bradley Hand, cursive;
            text-align: center;
          "
        >
          <h2>{{ product.title }}</h2>
          <figure>
            <img
              v-bind:src="product.image"
              style="height: 100px; width: 100px"
            />
          </figure>
          <p v-html="product.description"></p>
          <p>Price: {{ product.price }}</p>
          <p>
            Available Inventory:
            {{ product.availableInventory - cartCount(product.id) }}
          </p>
          <button @click="add(product)" v-if="canAddToCart(product)">
            Add To Cart
          </button>

          <button disabled="disabled" v-else>Add To Cart</button>
          <span v-if="product.availableInventory === 0"
            >All out!</span
          >
          <span
            v-else-if= "product.availableInventory - cartCount(product.id) < 10">
            Only {{ product.availableInventory - cartCount(product.id) }} left!
          </span>
          <span v-else> Buy now! </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script> /* eslint-disable */
export default {
  name: "ProductList",
  props: ["products", "cart"],

  data() {
    return {};
  },
  methods: {
    add(product) {
      console.log("added product", product.id);
      this.$emit("addProduct", product);
    },
    canAddToCart: function (product) {
      return product.availableInventory > 0;
    },
    cartCount(id) {
      let count = 0;
      if (this.cart && Array.isArray(this.cart)) {
        for (let i = 0; i < this.cart.length; i++) {
          if (this.cart[i] === id) {
            count++;
          }
        }
      }
      return count;
    },
  },
};
</script>



