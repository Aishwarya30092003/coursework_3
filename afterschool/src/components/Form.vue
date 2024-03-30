<template>
  <div>
    <h2>Checkout</h2>
    <h3 Added products></h3>
    <div v-for="product in cart" :key="product.id">
      {{ product.id }}
      <button @click="removeItem(product.id)">remove</button>
    </div>
    <div>
      <strong>Name</strong>
      <input v-model="name" />
      <strong> Address </strong>
      <input v-model="address" />
      <button @click="submitForm">Submit</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Form-component",
  props: ["cart", "products"],
  data() {
    return {
      name: "",
      address: "",
    };
  },

  methods: {
    removeItem(index) {
      console.log("Trying to remove product");
      console.log("Index: ", index);
      this.$emit("remove-item", index);
    },
    submitForm() {
      console.log("Form submitted with", this.name, this.address);
      this.name = "";
      this.address = "";
    },
  },
};
</script>
