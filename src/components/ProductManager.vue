<template>
  <div class="container mt-5">
    <h2>Product Manager</h2>
    <form @submit.prevent="submitProduct" class="mb-4">
      <input v-model="form.name" placeholder="Product Name" class="form-control mb-2" required />
      <input v-model="form.price" type="number" placeholder="Price" class="form-control mb-2" required />
      <button class="btn btn-success">{{ form.id ? 'Update' : 'Add' }} Product</button>
    </form>

    <ul class="list-group">
      <li v-for="product in products" :key="product.id" class="list-group-item d-flex justify-content-between">
        {{ product.name }} - ${{ product.price }}
        <div>
          <button @click="editProduct(product)" class="btn btn-warning btn-sm me-2">Edit</button>
          <button @click="deleteProduct(product.id)" class="btn btn-danger btn-sm">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      products: [],
      form: { id: null, name: '', price: '' }
    };
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    getProducts() {
      axios.get('http://localhost:3001/api/products')
        .then(res => this.products = res.data);
    },
    submitProduct() {
      if (this.form.id) {
        axios.put(`http://localhost:3001/api/products/${this.form.id}`, this.form)
          .then(() => {
            this.resetForm();
            this.getProducts();
          });
      } else {
        axios.post('http://localhost:3001/api/products', this.form)
          .then(() => {
            this.resetForm();
            this.getProducts();
          });
      }
    },
    editProduct(product) {
      this.form = { ...product };
    },
    deleteProduct(id) {
      axios.delete(`http://localhost:3001/api/products/${id}`)
        .then(() => this.getProducts());
    },
    resetForm() {
      this.form = { id: null, name: '', price: '' };
    }
  }
};
</script>
