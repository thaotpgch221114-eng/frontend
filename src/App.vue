<template>
  <div class="container">
    <table class="striped">
      <thead>
        <tr>
          <th colspan="5"><h3>Product List</h3></th>
        </tr>
        <tr>
          <th>Name</th>
          <th>Price</th>
          <th>Image</th>
          <th>Menu</th>
        </tr>
      </thead>
      <tbody v-if="data">
        <tr v-for="product in data" :key="product._id">
          <td>{{ product.name }}</td>
          <td>${{ product.price }}</td>
          <td>
            <img :src="'http://localhost:3000' + product.imageUrl" width="100" />
          </td>
          <td>
            <button @click="deleteProduct(product._id)" class="btn orange">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";

var backendAPI = import.meta.env.VITE_APP_API_URL;

export default {
  data() {
    return {
      data: null,
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    fetchProducts() {
      // Sẽ gọi: GET (đến địa chỉ trong biến VITE_APP_API_URL)
      axios
        .get(backendAPI)
        .then((response) => {
          this.data = response.data;
        })
        .catch((err) => {
          console.log("Error loading product list: " + err);
        });
    },
    deleteProduct(id) {
      const confirmed = confirm("Are you sure to delete this product?");
      if (confirmed) {
        // Sẽ gọi: DELETE (đến địa chỉ trong biến VITE_APP_API_URL)
        axios
          .delete(backendAPI + "/delete/" + id)
          .then(() => {
            this.fetchProducts();
          })
          .catch((err) => {
            console.error("Error deleting product:" + err);
          });
      }
    },
  },
};
</script>

<style>
.container {
  margin-top: 30px;
}
img {
  display: block;
  margin: auto;
}
</style>