<template>
    <div class="add-product">
        <button @click="$emit('add-product')" class="add-button">Ajouter produit</button>
    </div>
</template>

<script>
export default {
    name: 'AddProductButton'
};
</script>

<style scoped>
.add-product {
    text-align: center;
}

.add-button {
    padding: 5px 10px;
    border: revert;
    background-color: #81c6e0;
    color: white;
    border-radius: 25px;
    cursor: pointer;
}

.add-button:hover {
    background-color: #3d788e;
}
</style>