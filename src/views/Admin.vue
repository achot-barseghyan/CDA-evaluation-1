<template>
  <div>
    <h1>Admin page</h1>

    <div class="form">
      <h2>Create product</h2>
      <form action="">
        <label for="name">Name:</label>
        <input type="text" name="name" id="name" v-model="name" />
        <label for="price">Price:</label>
        <input type="number" name="price" id="price" v-model="price" />
        <label for="name">Stock:</label>
        <input type="number" name="stock" id="stock" v-model="stock" />
        <button v-if="formType === 'create'" class="btn" type="button" @click="submit">Create</button>
        <button v-if="formType === 'update'" class="btn" type="button" @click="handleUpdateSubmit">Update</button>
      </form>
    </div>

    <div class="table-wrapper">
      <table class="fl-table">
        <thead>
          <tr>
            <th>#</th>
            <th>name</th>
            <th>price</th>
            <th>stock</th>
            <th>actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in products" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.price }}</td>
            <td>{{ item.stock }}</td>
            <td>
              <button class="btn delete" @click="handleDelete(item.id)">
                Delete
              </button>
              <button class="btn" @click="handleUpdate(item.id)">Update</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      price: "",
      stock: "",
      products: [],
      formType: 'create',
      currentUpdateId: null
    };
  },
  methods: {
    async fetchProducts() {
      const res = await fetch("http://localhost:5000/products");
      const data = await res.json();
      return data;
    },
    submit() {
      let product = {
        name: this.name,
        price: this.price,
        stock: this.stock,
      };
      fetch("http://localhost:5000/products", {
        headers: {
          Accept: "application/json",

          "Content-Type": "application/json",
        },
        method: "POST",
        body: JSON.stringify(product),
      }).then(
        (this.name = ""),
        (this.price = ""),
        (this.stock = ""),
      );
    },
    handleDelete(id) {
      fetch("http://localhost:5000/products/" + id, {
        headers: { "content-type": "application/json" },
        method: "DELETE",
      })
        .then((res) => res.json())
        .then(() => {
          this.products.filter((product) => product.id !== id);
        })
        .catch((err) => console.log(err));
    },
    handleUpdate(id) {
        this.formType = 'update'
        let selectedProduct = this.products.find(product => product.id === id)
        this.name = selectedProduct.name
        this.price = selectedProduct.price
        this.stock = selectedProduct.stock
        this.currentUpdateId = id
    },
    handleUpdateSubmit() {
    let product = {
        name: this.name,
        price: this.price,
        stock: this.stock,
      };
      fetch("http://localhost:5000/products/"+ this.currentUpdateId, {
        headers: {
          Accept: "application/json",

          "Content-Type": "application/json",
        },
        method: "PUT",
        body: JSON.stringify(product),
      }).then(
        (this.name = ""),
        (this.price = ""),
        (this.stock = ""),
      );
    },
  },
  async created() {
    this.products = await this.fetchProducts();
  },
};
</script>

<style scoped>
.delete {
  background: rgb(163, 6, 6);
}
</style>