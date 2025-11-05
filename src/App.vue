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

// 1. Sửa backendAPI trỏ đến /product/api
var backendAPI = "http://localhost:3000/product/api";

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
      // Sẽ gọi: GET http://localhost:3000/product/api
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
        // 2. Sửa đường dẫn delete để gọi /api/delete/:id
        // Sẽ gọi: DELETE http://localhost:3000/product/api/delete/[id]
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