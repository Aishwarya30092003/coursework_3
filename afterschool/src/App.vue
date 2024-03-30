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
          :disabled="cartItemCount < 1"
        >
          {{ this.cart.length }}
        </button>
      </div>
    </header>
    <main>
      <product-list :products="products" @addProduct="addToCart"></product-list>

      <checkout :cart="cart"></checkout>
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
    showCheckout() {},
    addToCart(product) {
      console.log("addProduct event received by the root component.");
      this.cart.push(product);
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
