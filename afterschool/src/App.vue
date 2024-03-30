<template>
  <div class="app">
    <header>
      <div class="navbar">
        <div class="logo">
          <h1>{{ sitename }}</h1>
        </div>
        <div class="search-bar">
          <input
            type="text"
            placeholder="Search..."
            v-model="searchQuery"
            @input="filterProducts"
          />
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
    <div class="mainDiv" v-if="showProduct">
      <div class="sortDiv">
        <div class="filteringDiv">
          <p style="font-weight: bold">Sort By:</p>
          <br />
          <!-- Radio buttons for sorting options -->
          <label>
            <input type="radio" value="fees" v-model="filter" />Fees
            <br />
            <input type="radio" value="subject" v-model="filter" />Subject
            <br />
            <input type="radio" value="location" v-model="filter" />Location
            <br />
            <input type="radio" value="slots" v-model="filter" />Slots
          </label>
          <label>
            <p style="font-weight: bold">Sort Order:</p>
            <!-- Radio buttons for sorting order -->
            <input
              type="radio"
              value="ascending"
              v-model="sortOrder"
            />Ascending
            <br />
            <input
              type="radio"
              value="descending"
              v-model="sortOrder"
            />Descending
          </label>
        </div>
      </div>
      <product-list
        :products="sortedProducts"
        @addProduct="addToCart"
      ></product-list>
    </div>
    <div class="cartMainDiv" v-else>
      <checkout :cart="cart" @remove-item="deleteFromCart"> </checkout>
    </div>
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
      filter: "",
      sortOrder: "ascending",
      searchQuery: "",
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
      product.availableInventory--;
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
      return this.cart.length;
    },
    filteredProducts() {
      // Filter products based on search query
      const query = this.searchQuery.toLowerCase();
      return this.products.filter(
        (product) =>
          product.title.toLowerCase().includes(query) ||
          product.location.toLowerCase().includes(query)
      );
    },
    sortedProducts() {
      // Sort products based on selected filter and sortOrder
      let productsArray = this.filteredProducts.slice(0);
      switch (this.filter) {
        case "fees":
          productsArray.sort((a, b) =>
            this.sortOrder === "ascending" ? a.Fees - b.Fees : b.Fees - a.Fees
          );
          break;
        case "subject":
          productsArray.sort((a, b) =>
            this.sortOrder === "ascending"
              ? a.title.localeCompare(b.title)
              : b.title.localeCompare(a.title)
          );
          break;
        case "location":
          productsArray.sort((a, b) =>
            this.sortOrder === "ascending"
              ? a.location.localeCompare(b.location)
              : b.location.localeCompare(a.location)
          );
          break;
        case "slots":
          productsArray.sort((a, b) =>
            this.sortOrder === "ascending"
              ? a.availableInventory - b.availableInventory
              : b.availableInventory - a.availableInventory
          );
          break;
        default:
          break;
      }
      return productsArray;
    },
  },
};
</script>
