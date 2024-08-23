<template>
    <div v-if="visible" class="modal-overlay">
      <div class="modal-content">
        <button @click="$emit('close')" class="close-button">✖️</button>
        <h2>Search Products</h2>
        <input
          type="text"
          v-model="searchTerm"
          placeholder="Enter product name"
          @input="onSearch"
        />
        <ul v-if="searchResults.length > 0">
          <li v-for="product in searchResults" :key="product.id" class="search-result-item">
            <img :src="product.photo" alt="Product image" class="product-image"/>
            <div class="product-details">
              <p>{{ product.nom }}</p>
              <p>Qty: {{ product.qte }}</p>
            </div>
          </li>
        </ul>
        <p v-else>No products found.</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'SearchProductModal',
    props: {
      visible: Boolean
    },
    data() {
      return {
        searchTerm: '',
        searchResults: []
      };
    },
    methods: {
      onSearch() {
        if (this.searchTerm.trim()) {
          fetch(`http://apifrigo.pecatte.fr/api/1/produits?search=${encodeURIComponent(this.searchTerm)}`)
            .then(response => response.json())
            .then(data => {
              this.searchResults = data;
            })
            .catch(error => console.error('Error searching products:', error));
        } else {
          this.searchResults = [];
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.7);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  
  .modal-content {
    background: white;
    padding: 20px;
    border-radius: 10px;
    width: 400px;
    max-width: 80%;
    position: relative;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
  }
  
  .close-button {
      position: absolute;
      top: 10px;
      right: 10px;

      background-color: #e74c3c;
      color: white;
      border: none;
      border-radius: 3px;
      cursor: pointer;
      font-size: 14px;
      margin: 0 2px;
  }
  
  input[type="text"] {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    box-sizing: border-box;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  .search-result-item {
    display: flex;
    align-items: center;
    padding: 10px;
    border-bottom: 1px solid #ddd;
  }
  
  .product-image {
    width: 100px;
    height: auto;
    margin-right: 10px;
    border-radius: 5px;
  }
  
  .product-details {
    flex: 1;
  }
  
  p {
    margin: 0;
  }
  
</style>
  