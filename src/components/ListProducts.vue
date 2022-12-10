<template>
  <div class="container">
    <template v-if="!loading">
      <h1>List Products</h1>
      <div v-if="error" class="alert alert-danger" role="alert">
          Error with the application
        </div>
    <table class="table" v-if="!error">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Title</th>
          <th scope="col">Price</th>
          <th scope="col">Category</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td>{{ product.id }}</td>
          <td> {{ product.title }}</td>
          <td>{{ product.price }}</td>
          <td>{{ product.category }}</td>
        </tr>
      </tbody>
    </table>
    </template>
      <div v-else class="d-flex justify-content-center">
        <div class="spinner-border" role="status">
          <span class="sr-only"></span>
        </div>
      </div>  
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'ListProducts',
  props: {
    msg: String
  },
  data() {
      return {
         products: {},
         loading: false,
         error: false
      }
  },
  async created() {
      await this.listProducts();
  },
  methods: {
   async listProducts() {
        this.loading = true;
        try {
          const productsAPi = await axios.get('https://fakestoreapi.com/products'); 
          let dataProduct = productsAPi.data;
          this.products = dataProduct.map(function(element){
            return {
                id: element.id,
                title: element.title,
                price: element.price,
                category: element.category
            }
        }).sort((a,b) => b.category - a.category);
        } catch(error) {
          this.loading = false;
          this.error = true;
        } finally {
          this.loading = false;
       }
    }
  }
}
</script>