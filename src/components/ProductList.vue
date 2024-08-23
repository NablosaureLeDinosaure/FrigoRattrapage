<template>
  <div>
    <FridgeHeader />
    
    <div class="fridge-container">
      <div v-for="(product, index) in fridgeSlots" :key="index" class="fridge-slot">
        <ProductItem
          v-if="product"
          :product="product"
          @update-quantity="updateProductQuantity"
          @remove="removeProduct"
        />
        <AddProductButton v-else @add-product="openAddProductModal(index)" />
      </div>
    </div>
    <SearchButton @click="showSearchModal = true" />
    <FridgeFullMessage v-if="isFridgeFull" />
    <AddProductModal
      :visible="showAddProductModal"
      @add-product="addProduct"
      @close="closeAddProductModal"
    />
    <SearchProductModal
      :visible="showSearchModal"
      @close="closeSearchModal"
    />
  </div>
</template>

<script>
import FridgeHeader from './FridgeHeader.vue';
import ProductItem from './ProductItem.vue';
import AddProductButton from './AddProductButton.vue';
import AddProductModal from './AddProductModal.vue';
import FridgeFullMessage from './FridgeFullMessage.vue';
import SearchProductModal from './SearchProductModal.vue';
import SearchButton from './SearchButton.vue';

export default {
  name: 'ProductList',
  components: {
    FridgeHeader,
    ProductItem,
    AddProductButton,
    AddProductModal,
    FridgeFullMessage,
    SearchProductModal,
    SearchButton
  },
  data() {
    return {
      products: [],
      maxSlots: 8,
      showAddProductModal: false,
      showSearchModal: false,
      selectedSlotIndex: null
    };
  },
  computed: {
    fridgeSlots() {
      const slots = Array(this.maxSlots).fill(null);
      for (let i = 0; i < this.products.length; i++) {
        slots[i] = this.products[i];
      }
      return slots;
    },
    isFridgeFull() {
      return this.products.length >= this.maxSlots;
    }
  },
  methods: {
    fetchProducts() {
      fetch('http://apifrigo.pecatte.fr/api/1/produits')
        .then(response => response.json())
        .then(data => {
          this.products = data;
        })
        .catch(error => console.error('Error fetching products:', error));
    },
    updateProductQuantity(product, newQuantity) {
      product.qte = newQuantity;
      this.updateProduct(product);
    },
    updateProduct(product) {
      fetch('http://apifrigo.pecatte.fr/api/1/produits', {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          id: product.id,
          nom: product.nom,
          qte: product.qte,
          photo: product.photo
        })
      })
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then(data => {
          if (data.status === 1) {
            console.log('Product updated successfully');
            this.fetchProducts(); // Refresh products after update
          } else {
            console.error('Failed to update product');
          }
        })
        .catch(error => {
          console.error('Error updating product:', error);
        });
    },
    removeProduct(product) {
      fetch(`http://apifrigo.pecatte.fr/api/1/produits/${product.id}`, {
        method: 'DELETE'
      })
        .then(response => response.json())
        .then(data => {
          if (data.status === 1) {
            console.log('Product removed successfully');
            this.fetchProducts(); // Refresh products after removal
          } else {
            console.error('Failed to remove product');
          }
        })
        .catch(error => {
          console.error('Error removing product:', error);
        });
    },
    openAddProductModal(index) {
      this.selectedSlotIndex = index;
      this.showAddProductModal = true;
    },
    closeAddProductModal() {
      this.showAddProductModal = false;
      this.selectedSlotIndex = null;
    },
    addProduct(newProduct) {
      const defaultImage = 'https://cdn-icons-png.flaticon.com/512/3593/3593455.png';

      function isValidHTTPURL(url) {
        try {
          const urlObject = new URL(url);
          return urlObject.protocol === 'http:' || urlObject.protocol === 'https:';
        } catch (_) {
          return false;
        }
      }

      const productData = {
        nom: newProduct.nom.trim(),
        qte: parseInt(newProduct.qte, 10),
        photo: isValidHTTPURL(newProduct.photo.trim()) ? newProduct.photo.trim() : defaultImage
      };

      console.log('Adding product with data:', JSON.stringify(productData));

      fetch('http://apifrigo.pecatte.fr/api/1/produits', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(productData)
      })
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then(data => {
          if (data.status === 1) {
            console.log('Product added successfully');
            this.fetchProducts(); // Refresh products after adding
          } else {
            console.error('Failed to add product');
          }
        })
        .catch(error => {
          console.error('Error adding product:', error);
        });
    },
    closeSearchModal() {
      this.showSearchModal = false;
    }
  },
  mounted() {
    this.fetchProducts();
  }
};
</script>

<style>
.fridge-container {
  display: flex;
  flex-wrap: wrap;
  width: 27%;
  height: 57%;
  position: relative;
  justify-content: space-between;
  margin-left: 0.4rem;
}

.fridge-slot {
  width: 37%;
  display: inherit;
  justify-content: center;
  align-items: center;
  height: 11%;
  box-sizing: border-box;
  margin-bottom: 1.2rem;
  align-content: center;
}
</style>
