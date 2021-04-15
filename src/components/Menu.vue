<template>
  <div class="ui secondary menu">
    <div class="ui container">
      <div class="left menu">
        <router-link class="item" to="/">
          <img
            class="ui small image"
            src="../assets/logo.png"
            alt="Ecommerce"
          />
           <div v-for="category in categories" :key="category.id">
            <router-link class="item" :to="category.slug">
              {{ category.title }}
            </router-link>
          </div>
        </router-link>
      </div>

      <div class="right menu">
        <router-link class="item" to="/login" v-if="!token">
          Iniciar sesión
        </router-link>
        <template v-if="token">
          <router-link class="item" to="/orders">Pedidos</router-link>
          <span class="ui item cart" @click="openCart">
            <i class="shopping cart icon"></i>
          </span>
          <span class="ui item logout" @click="logout">
            <i class="sign-out icon"></i>
          </span>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import {getTokenApi,deleteTokenApi} from "../api/token.js"
import {ref,onMounted} from "vue"
import {getCategoriesApi} from "../api/category.js"
 import {useStore} from "vuex"
export default {
  name: "BasicLayout",
  components: {
  },
  setup(){
    const token = getTokenApi();
    let categories = ref(null);
    const store = useStore();

    const openCart = () => {
      store.commit("setShowCart", true);
      console.log("si")
      console.log("si")
    };

    onMounted(async () => {
      const response = await getCategoriesApi();
      categories.value=response;
    });
 
    const logout = ()=>{
      console.log("Cerrar sesion")
      deleteTokenApi();
      location.replace("/")
    }

    return {
      token,
      logout,
      categories,
      openCart
    }
  }
};
</script>

<style lang="scss">
.ui.menu.secondary {
  background-color: #16202b !important;

  .item {
    color: white;
    &:hover {
      color: #1fa1f1;
    }
  }
  .menu.left {
    width: 50%;
    .ui.image {
      width: 40px;
    }
  }
  .menu.right {
    width: 50%;
    justify-content: flex-end;
    .logout,
    .cart {
      &:hover {
        cursor: pointer;
      }
    }
  }
}
</style>
