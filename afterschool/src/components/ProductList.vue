<template>
  <div>
    <div class="productMainDiv">
      <div class="products">
        <div
          class="productDetails"
          v-for="product in products"
          :key="product.id"
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
          <p>Available stock:{{ product.availableInventory }}</p>
          <button
            class="addToCart"
            @click="add(product)"
            v-if="canAddToCart(product)"
          >
            Add To Cart
          </button>
          <button
            style="opacity: 0.5"
            class="addToCart"
            disabled="disabled"
            v-else
          >
            Add To Cart
          </button>
          <!-- Messages for inventory status -->
          <span v-if="product.availableInventory === 0">All Out!</span>
          <span v-else-if="product.availableInventory - cartCount(product.id)">
            Only {{ product.availableInventory }} left!
          </span>
          <span v-else>Buy Now!!</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductList",
  props: ["products", "cart"], // Assuming cart is a prop

  data() {
    return {};
  },
  methods: {
    add(product) {
      console.log("added product", product.id);
      this.$emit("addProduct", product);
    },
    canAddToCart(product) {
      // Check if the product can be added to the cart
      return product.availableInventory > this.cartCount(product.id);
    },
    cartCount(id) {
      // Count the number of instances of a product in the cart
      let count = 0;
      if (this.cart) {
        for (let i = 0; i < this.cart.length; i++) {
          if (this.cart[i] === id) {
            count++;
          }
        }
      }
      return count;
    },
    filterProducts() {
      const query = this.searchQuery.toLowerCase();
      this.filteredProducts = this.sortedProducts.filter(
        (product) =>
          product.title.toLowerCase().includes(query) ||
          product.location.toLowerCase().includes(query)
      );
    },
  },
};
</script>
