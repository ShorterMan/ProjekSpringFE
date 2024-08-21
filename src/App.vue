<template>

  <div>
    <h3>Product List</h3>
    <ProductList @order="addOrderData"/>

    <h3>Order Cart</h3>
    <OrderCart ref="orderCart"/>
  </div>
</template>

<script>
import ProductList from './components/ProductList'
import OrderCart from "./components/OrderCart"

export default {
  name: 'App',
  components: {
    ProductList,
    OrderCart
  },
  methods:{
    addOrderData(data){
      this.$refs.orderCart.addOrderData(data);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
