<template>
  <div class="cartMainDiv">
    <div class="cart-items">
      <div v-for="product in cart" :key="product.id" class="cartProducts">
        <figure>
          <img v-bind:src="product.image" height="100px" width="100px" />
        </figure>
        <h2 v-text="product.title"></h2>
        <p v-text="product.description"></p>
        <p>Fees: {{ product.Fees }}</p>
        <button class="removeButton" @click="removeItem(product.id)">remove</button>
      </div>
    </div>
    <div>
      <div class="order-info" style="color: white; margin-top: 20px">
        <h2>Order Information</h2>
        <label for="firstName">First Name:</label>
        <input
          type="text"
          id="firstName"
          v-model="order.firstName"
          @input="validateOrderForm"
        />
        <br /><br />
        <label for="lastName">Last Name:</label>
        <input
          type="text"
          id="lastName"
          v-model="order.lastName"
          @input="validateOrderForm"
        />
        <br /><br />
        <label for="phone">Phone:</label>
        <input
          type="text"
          id="phone"
          v-model="order.phone"
          @input="validateOrderForm"
        />
        <br /><br />
        <!-- Button to place the order with conditional disabled state -->
        <button @click="submitForm" :disabled="!isOrderFormValid">
          Place Order
        </button>
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
        phone: "",
      },
    };
  },

  methods: {
    removeItem(index) {
      console.log("Trying to remove product");
      console.log("Index: ", index);
      this.$emit("remove-item", index);
    },
    submitForm() {
      alert("Order Submitted!");
    },
    validateOrderForm() {
      const nameRegex = /^[A-Za-z]+$/;
      const phoneRegex = /^[0-9]+$/;

      const isNameValid =
        nameRegex.test(this.order.firstName) &&
        nameRegex.test(this.order.lastName);
      const isPhoneValid = phoneRegex.test(this.order.phone);

      this.isOrderFormValid = isNameValid && isPhoneValid;
    },
  },
};
</script>
