<template>
  <div>
    <h2>Checkout</h2>
    <h3 Added products></h3>
    <div v-for="(product, index) in cart" :key="product.id">
      {{ product.id }}
      <button @click="removeItem(index)">remove</button>
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
  props: ['cart'],
  data() {
    return {
      name: "",
      address: "",
    };
  },

  methods: {
    removeItem(index) {
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
