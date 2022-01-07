<template>
  <div>
    <h1>Cart</h1>
    <div class="table-wrapper">
      <table class="fl-table">
        <thead>
          <tr>
            <th>#</th>
            <th>name</th>
            <th>price</th>
            <th>quantity</th>
            <th>total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in products" :key="item.product.id">
            <td>{{ item.product.id }}</td>
            <td>{{ item.product.name }}</td>
            <td>{{ item.product.price }}</td>
            <td class="quantity">
              {{ item.quantity }}
              <div>
                <button class="btn">+</button>
                <button class="btn">-</button>
              </div>
            </td>
            <td>{{ item.quantity * item.product.price }} €</td>
          </tr>
        </tbody>
      </table>
    </div>
    <h2>Total : {{ totalOrder }} €</h2>
    <button class="btn" @click="valideOrder">Valide Order</button>
  </div>
</template>

<script>
export default {
  name: "Cart",
  data() {
    return {
      products: [],
      cartInLocalStorage: [],
      totalOrder: 0,
    };
  },
  methods: {
    async fetchProducts() {
      const res = await fetch("http://localhost:5000/products");
      const data = await res.json();
      return data;
    },
    getProductsInCart() {
      this.cartInLocalStorage.forEach((cartItem) => {
        fetch(`http://localhost:5000/products/${cartItem.id}`)
          .then((response) => response.json())
          .then((product) => {
            this.products.push({
              product: product,
              quantity: cartItem.quantity,
            });
            this.totalOrder += product.price * cartItem.quantity;
          });
      });
      console.log(this.products);
    },
    valideOrder() {

       let order = {
           order: this.products,
       } 

      fetch("http://localhost:5000/orders", {
        headers: {
          Accept: "application/json",

          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(order),
      }).then(localStorage.clear())
    },
  },
  async created() {
    this.cartInLocalStorage = JSON.parse(localStorage.getItem("cart"));
    this.getProductsInCart();
  },
};
</script>

<style scoped>
.quantity {
  display: flex;
  align-items: center;
  justify-content: center;
}

.quantity .btn {
  padding: 0 0.3rem;
}
</style>