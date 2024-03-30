<template>
  <div>
    <h2>Checkout</h2>
    <h3 Added products></h3>
    <div class="cart-items">
      <div
        v-for="product in cart"
        :key="product.id"
        style="
          margin: 10%;
          background-color: #cce3de;
          padding: 2%;
          border-radius: 2%;
          width: 20%;
        "
      >
        <figure>
          <img v-bind:src="product.image" height="100px" width="100px" />
        </figure>
        <h2 v-text="product.title"></h2>
        <p v-text="product.description"></p>
        <br />
        <p>Price: {{ product.price }}</p>
        <br />
        <p>Location: {{ product.location }}</p>

        <br />
        <button @click="removeItem(product.id)">remove</button>
      </div>
      <div>
        <div
          class="checkout"
          style="
            margin: 2%;
            background-color: #a3b18a;
            padding: 2%;
            border-radius: 2%;
            width: 80%;
          "
        >
          <h2 style="text-align: center">Checkout</h2>
          <p>
            <strong>First Name: </strong>
            <input v-model.trim="order.firstName" @input="validation" />
          </p>
          <p>
            <strong>Last Name: </strong>
            <input v-model.trim="order.lastName" @input="validation" />
          </p>
          <p>
            <strong>Address: </strong>
            <input v-model="order.address" />
          </p>
          <p>
            <strong>City: </strong>
            <input v-model="order.city" />
          </p>

          <p>
            <strong>Zip/Postal Code: </strong>
            <input v-model.number="order.zip" type="number" />
          </p>

          <p>
            <strong>Phone Number: </strong>
            <input
              v-model.number="order.phoneNumber"
              type="number"
              @input="validation"
            />
          </p>
          <p>
            <input
              type="checkbox"
              id="gift"
              value="true"
              v-model="order.gift"
              v-bind:true-value="order.sendGift"
              v-bind:false-value="order.dontSendGift"
            />
            <label for="gift">Ship as Gift?</label>
          </p>
          <p>
            <input type="radio" id="home" value="Home" v-model="order.method" />
            <label for="home">Home</label>
            <input
              type="radio"
              id="business"
              value="Business"
              v-model="order.method"
            />
            <label for="business">Business</label>
          </p>
          <button
            @click="placeOrder"
            :disabled="!isOrderFormValid"
            style="justify-content: center"
          >
            Place Order
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Form-component",
  props: ["cart", "products"],
  data() {
    return {
      order: {
        firstName: "",
        lastName: "",
        address: "",
        city: "",
        zip: "",
        phoneNumber: "",
        method: "Home",
        gift: false,
        sendGift: "send as gift",
        dontSendGift: "dont send as a gift",
      },
    };
  },

  methods: {
    removeItem(index) {
      console.log("Trying to remove product");
      console.log("Index: ", index);
      this.$emit("remove-item", index);
    },
    placeOrder: function () {
      this.orderPlaced = true;
      alert("Order Submitted!!!!");
    },

    validation() {
      const nameRegex = /^[A-Za-z]+$/;
      const phoneRegex = /^[0-9]+$/;

      const isNameValid =
        nameRegex.test(this.order.firstName) &&
        nameRegex.test(this.order.lastName);
      const isPhoneValid = phoneRegex.test(this.order.phoneNumber);

      this.isOrderFormValid = isNameValid && isPhoneValid;
    },
  },
};
</script>



