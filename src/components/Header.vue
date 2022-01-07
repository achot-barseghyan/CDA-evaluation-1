<template>
    <div id="nav">
      <div>
        <router-link to="/">Home</router-link>
        <router-link to="/admin">Admin</router-link>
      </div>
        <div>
          <router-link class="btn" to="/cart">Panier</router-link>
          <span v-if="cart.length > 0">{{cart.length}} items</span>
        </div>
    </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      cart: []
    }
  },
  created() {
    if (localStorage.getItem('cart')) this.cart = JSON.parse(localStorage.getItem('cart'))
  }
};
</script>

<style scoped>
#nav{
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0 20%;
  padding: 0 2rem;
  background-color: rgb(194, 194, 194);
  border-radius: 20px;
}
</style>