<template>
  <BasicLayout>
    <h1>Carrito</h1>
    <table class="ui celled table" v-if="products">
      <thead>
        <tr>
          <th>Producto</th>
          <th>Cantidad</th>
          <th>Precio</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td>{{ product.name }}</td>
          <td>{{ product.quantity }}</td>
          <td>$ {{ product.price }}</td>
          <td style="text-align:center">
            <i class="close icon" @click="deleteAllProductCart(product.id)"></i>
          </td>
        </tr>
        <tr>
          <td></td>
          <td>Total:</td>
          <td colspan="2">$ {{ getTotal() }}</td>
        </tr>
      </tbody>
    </table>

    <button class="ui button primary fluid" v-if="products" @click="createOrder">
      Generar pedido
    </button>
    <h3 v-if="!products">No tienes productos en el carrito</h3>
  </BasicLayout>
</template>
<script>
import BasicLayout from "../layouts/BasicLayout";
import { getProductsCardApi,deleteAllProductCartApi, deleteProductCartApi,deleteCartApi } from "../api/cart.js";
import { ref, watchEffect } from "vue";
import {createOrderApi} from "../api/order.js"
import {getTokenApi} from "../api/token.js"
import jwtDecode from "jwt-decode"
import {useRouter, useRoute} from "vue-router"
export default {
  name: "Cart",
  components: {
    BasicLayout,
  },
  setup() {
    let products = ref(null);
    let reloadCart=ref(false)
    const router = useRouter()
    watchEffect(async () => {
      reloadCart.value;
      const response = await getProductsCardApi();
      products.value = response;
    });

    const deleteAllProductCart=(idProduct)=>{
        deleteAllProductCartApi(idProduct)
        reloadCart.value=!reloadCart.value;
    }

    const createOrder= async()=>{
        const token = getTokenApi();
        console.log(token)
        const user = jwtDecode(token)
        console.log(user.id)
        const data ={
            user:user.id,
            totalPayment:getTotal(),
            data:products.value
        }
        try {
            const response = await createOrderApi(data)
            console.log("pedido")
            deleteCartApi();
            router.push("/orders")
            router
        } catch (error) {
            console.log(error)
        }
    }

    const getTotal = () => {
      let totalTemp = 0;
      products.value.forEach((product) => {
        totalTemp += product.price * product.quantity;
      });
      return totalTemp.toFixed(2);
    };

    return {
      products,
      getTotal,
      deleteAllProductCart,
      createOrder
    };
  },
};
</script>
<style lang=""></style>
