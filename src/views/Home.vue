<template>
  <div class="home">
    <h1>Home</h1>
    <div class="cards">
      <Card :products='products' @clicked="onClickChild"/>
    </div>
  </div>
</template>

<script>

import Card from '../components/Card.vue'

export default {
  name: 'Home',
  components: {
    Card
  },
  data() {
    return {
      products : [],
      cartLocal: [],
      productFromStorage : null,
    }
  },
  methods: {
    async fetchProducts() {
      const res = await fetch('http://localhost:5000/products')
      const data = await res.json()
      return data
    },
    onClickChild(id) {
      let cartInLocalStorage = JSON.parse(localStorage.getItem('cart'))

      if (cartInLocalStorage != null) {
        this.productFromStorage = cartInLocalStorage.find(product => product.id === id)
      }

      if (this.productFromStorage === undefined || this.productFromStorage === null) {
        this.cartLocal.push({id: id, quantity: 1})
        localStorage.setItem('cart', JSON.stringify(this.cartLocal))
      }else{
        alert("product already in your cart !")
      }
    }
  },
  async created() {
    this.products = await this.fetchProducts()
  }
}
</script>

<style scoped>
  .cards {
    display: flex;
    flex-wrap: wrap;
  }
</style>
