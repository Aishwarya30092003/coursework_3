<template>
  <div class="app">
    <header>
      <div class="navbar">
        <div class="logo">
          <h1>{{ sitename }}</h1>
        </div>
        <button
          class="fas fa-shopping-cart"
          @click="showCheckout"
          :disabled="cartItemCount === 0"
        >
          {{ this.cart.length }}
        </button>
      </div>
    </header>
    <main>
      <product-list
        :products="products"
        @addProduct="addToCart"
        v-if="showProduct"
      ></product-list>

      <checkout :cart="cart" @remove-item="deleteFromCart" v-else></checkout>
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
      sitename: "After School Hub",
      showProduct: true,
      cart: [],
      products: [], // Ensure products is properly initialized
    };
  },

  created() {
    console.log("requesting data from server");
    // Fetch initial product data from server
    fetch("http://localhost:3000/collection/Products").then((response) => {
      response.json().then((json) => {
        this.products = json; // Assign fetched data to products
        console.log(json);
      });
    });
  },
  methods: {
    showCheckout: function () {
      // Toggle between displaying products and the shopping cart
      this.showProduct = !this.showProduct;
    },
    addToCart(product) {
      console.log("addProduct event received by the root component.");
      this.cart.push(product);
    },
    deleteFromCart(id) {
      // Find the index of the product with the given ID
      const index = this.cart.findIndex((product) => product.id === id);

      // If the product is found, remove it from the cart
      if (index !== -1) {
        this.cart.splice(index, 1);
      }
    },
  },
  computed: {
    cartItemCount: function () {
      // Calculate the total number of items in the cart
      return this.cart.length || "";
    },
  },
};
</script>
