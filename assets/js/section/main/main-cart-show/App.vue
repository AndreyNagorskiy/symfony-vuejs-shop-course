<template>
  <div class="row">
    <div class="col-lg-12 order-block">
      <div class="order-content">
        <Alert />
        <div v-if="showCartContent">
          <CartProductList />
          <CartTotalPrice />
          <div v-if="isNotEmptyCart">
            <a
                v-if="isUserLoggedIn"
                class="btn btn-success mb-3 text-white"
                @click="makeOrder"
            >
              MAKE ORDER
            </a>
            <a
                v-else
                class="btn btn-success mb-3 text-white"
                @click="redirectToLoginPage"
            >
              SIGN IN
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CartProductList from "./components/CartProductList";
import CartTotalPrice from "./components/CartTotalPrice";
import {mapActions, mapState} from "vuex";
import Alert from "./components/Alert";
export default {
  name: "App",
  components: {Alert, CartTotalPrice, CartProductList},
  created() {
    this.getCart();
  },
  computed: {
    ...mapState("cart", ["cart", "isSentForm", "staticStore"]),
    showCartContent() {
      return !this.isSentForm && Object.keys(this.cart).length;
    },
    isNotEmptyCart() {
      return this.cart.cartProducts.length;
    },
    isUserLoggedIn() {
      return this.staticStore.user.isLoggedIn;
    },
  },
  methods: {
    ...mapActions("cart", ["getCart", "makeOrder"]),
    redirectToLoginPage() {
      window.open(this.staticStore.url.loginPage).focus();
    }
  }
}
</script>

<style scoped>

</style>