<template>
  <div>
    <div class="basic-layout">
      <Menu />
      <div class="ui container">
        <slot />
      </div>
    </div>
    <Cart />
  </div>
</template>
<script>
import Menu from "../components/Menu";
import Cart from "../components/Cart/Cart";
export default {
  name: "BasicLayout",
  components: {
    Menu,
    Cart,
  },
};
</script>
