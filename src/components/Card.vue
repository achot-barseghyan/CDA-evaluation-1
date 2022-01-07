<template>
  <div v-for="product in products" :key="product.id" class="card">
    <div class="header">
        Plat
    </div>
    <div class="container-card">
      <p>Name: {{ product.name }}</p>
      <p>Price: {{ product.price }}</p>
    </div>
    <div class="buttons">
        <button v-if="product.stock > 0" class="btn" @click="addProductToCart(product.id)">Add to cart</button>
        <p v-else class="no-stock"> No stock for this product</p>
    </div>
    
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    products: Array,
  },
  data() {
      return {
      }
  },
  methods: {
    addProductToCart (id) {
      this.$emit('clicked', id)
    }
  }
};
</script>

<style scoped>
.card {
  width: 20%;
  display: flex;
  flex-direction: column;
  border: 1px rgb(0, 0, 0) solid;
}

.header {
  height: 30%;
  background: rgb(0, 0, 0);
  color: white;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  max-height: 1rem;
  padding: 1rem;
}
.container-card {
  padding: 2px 16px;
}

.btn{
    text-transform: uppercase;
}

.no-stock {
    color: rgb(165, 0, 0);
    text-transform: uppercase;
}
</style>