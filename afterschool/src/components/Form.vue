<template>
  <div>
    <h2>Checkout</h2>
    <h3 Added products></h3>
    <div v-for="product in cart" :key="product.id">
      {{ product.id }}
      <button @click="removeItem(product.id)">remove</button>
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
