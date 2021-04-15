<template>
  <div class="cart-body" v-if="products">
    <div class="product" v-for="product in products" :key="product.id">
      <img :src="API_URL + product.image.url" :alt="product.name" />
      <div class="info">
        <p>{{ product.name }}</p>
        <div class="price">
          <p>$ {{ product.price }}</p>
          <div class="quantity">
            <button class="ui button primary" size="large" @click="increaseProductCart(product.id)">+</button>
            <p>{{ product.quantity }}</p>
            <button class="ui button primary" size="large" @click="decreaseProductCart(product.id)">-</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { API_URL } from "../../utils/constants.js";
import {addProductCartApi,deleteProductCartApi} from "../../api/cart.js"
export default {
  name: "CardBody",
  props: {
    products: Array,
    reloadCartFn:Function
  },
  setup(props) {
    const increaseProductCart=(id)=>{
      console.log(id)
      addProductCartApi(id);
      props.reloadCartFn();

    }

    const decreaseProductCart=(id)=>{
      deleteProductCartApi(id);
      props.reloadCartFn();
    }

    return {
      API_URL,
      increaseProductCart,
      decreaseProductCart
    };
  },
};
</script>

<style lang="scss" scoped>
.cart-body {
  padding: 20px 10px;
  overflow-y: scroll;
  height: calc(100vh - 50px - 90px);
  &::-webkit-scrollbar {
    display: none;
  }

  .product {
    display: flex;
    img {
      width: 250px;
      margin-bottom: 10px;
    }
    .ui.image.fluid{
        width:100px !important;
    }
    .info{
        display:flex;
        flex-direction:column;
        justify-content:center;
        width:100%;
    }
    .price{
        display:flex;
        justify-content:space-between;
        p{
            margin:0;
        }
    }
    .quantity{
        display:flex;
        align-items:center;
        p{
            margin:0 10px;
        }
        button{
            padding: 4px 10px;
        }
    }
  }
}
</style>
