<template>
  <v-app>
    <v-main>
      <v-container fluid class="fridge-background">
        <ProductList />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import ProductList from './components/ProductList.vue';

export default {
  name: 'App',
  components: {
    ProductList
  }
};
</script>

<style>
.fridge-background {
  background-image: url('../public/frigoImage2.jpg');
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  height: 100vh;
  display: flex;
  align-items: normal;
  justify-content: center;
  text-align: -webkit-center;
}
</style>
