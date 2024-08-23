<template>
  <div class="product-item">
    <div class="product-content">
      <div class="product-info">
        <p>{{ product.nom }}</p>
        <p>Qty: {{ product.qte }}</p>
        <div class="buttons-container">
          <button @click="increaseQuantity" class="quantity-button">+</button>
          <button @click="decreaseQuantity" class="quantity-button">-</button>
          <button @click="removeProduct" class="remove-button">✖️</button>
        </div>
      </div>
      <img :src="product.photo" alt="product.nom" class="product-image" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: Object
  },
  methods: {
    increaseQuantity() {
      this.$emit('update-quantity', this.product, this.product.qte + 1);
    },
    decreaseQuantity() {
      if (this.product.qte > 0) {
        const newQuantity = this.product.qte - 1;
        this.$emit('update-quantity', this.product, newQuantity);

        // Supprime automatiquement le produit si la quantité atteint 0
        if (newQuantity === 0) {
          this.removeProduct();
        }
      }
    },
    removeProduct() {
      this.$emit('remove', this.product);
    }
  }
};
</script>

<style scoped>
.product-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.product-content {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.product-image {
  width: 65%;
  height: auto;
  margin-left: 7%;
  border-radius: 10px;
}

.product-info {
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 50%;
  width: 104%;
  /* Ajuster la largeur */
  word-wrap: break-word;
  text-align: center;
  margin-left: -19%;
}

.buttons-container {
  display: flex;
  justify-content: space-between;
  margin-top: 5px;
}

.quantity-button {
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 3px;
  padding: 5px 8px;
  cursor: pointer;
  font-size: 14px;
  margin: 0 2px;
}

.quantity-button:hover {
  background-color: #2980b9;
}

.remove-button {
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 3px;
  padding: 5px 8px;
  cursor: pointer;
  font-size: 14px;
  margin: 0 2px;
}

.remove-button:hover {
  background-color: #c0392b;
}
</style>
