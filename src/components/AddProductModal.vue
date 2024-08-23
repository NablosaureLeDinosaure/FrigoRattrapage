<template>
    <div class="modal" v-if="visible">
      <div class="modal-content">
        <span class="close" @click="close">&times;</span>
        <h2>Ajouter un produit</h2>
        <form @submit.prevent="submitForm">
          <div class="form-group">
            <label for="nom">Nom du produit :</label>
            <input type="text" v-model="newProduct.nom" id="nom" required />
          </div>
          <div class="form-group">
            <label for="qte">Quantité :</label>
            <input type="number" v-model.number="newProduct.qte" id="qte" required min="1" />
          </div>
          <div class="form-group">
            <label for="photo">URL de la photo :</label>
            <input type="text" v-model="newProduct.photo" id="photo" />
          </div>
          <button type="submit" class="submit-button">Ajouter</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AddProductModal',
    props: {
      visible: Boolean
    },
    data() {
      return {
        newProduct: {
          nom: '',
          qte: 1,
          photo: ''
        }
      };
    },
    methods: {
      submitForm() {
        this.$emit('add-product', { ...this.newProduct });
        this.close();
      },
      close() {
        this.$emit('close');
        this.resetForm();
      },
      resetForm() {
        this.newProduct = {
          nom: '',
          qte: 1,
          photo: ''
        };
      }
    }
  };
  </script>
  
  <style scoped>
  .modal {
    display: flex;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    justify-content: center;
    align-items: center;
  }
  
  .modal-content {
    background-color: #fefefe;
    padding: 20px;
    border: 1px solid #888;
    width: 400px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  }
  
  .close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
    cursor: pointer;
  }
  
  .close:hover,
  .close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  .submit-button {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 5px;
  }
  </style>
  