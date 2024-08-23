<template>
    <button class="search-button" @click="$emit('click')">
      Rechercher un produit
    </button>
  </template>
  
  <script>
  export default {
    name: 'SearchButton'
  };
  </script>
  
  <style scoped>
  .search-button {
    padding: 10px 20px;
    border: none;
    background-color: #007bff;
    color: white;
    border-radius: 5px;
    cursor: pointer;
    margin-bottom: 10px;
    display: block;
    margin-left: auto;
    margin-right: auto;
  }
  
  .search-button:hover {
    background-color: #0056b3;
  }
  </style>
  