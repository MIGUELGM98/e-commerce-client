<template>
  <BasicLayout>
    <h1>Ultimos productos</h1>
    <div class="ui grid">
      <div class="sixten wide mobile eight wide tablet four wide computer column"
        v-for="product in products" :key="product.id"
      >
      <Product :product="product"/>

      </div>
    </div>

  </BasicLayout>
</template>

<script>
import BasicLayout from "../layouts/BasicLayout"
import Product from "../components/Product"
import {getProducts} from "../api/product.js"
import {ref, onMounted} from "vue"
export default {
  name: "Home",
  components: {
    BasicLayout,
    Product
  },
  setup(){
    let products = ref(null);
    onMounted(async() => {
      const response = await getProducts(20);
      console.log(response)
      products.value=response;
    });


    return{
      products,
    };
  }
};
</script>
