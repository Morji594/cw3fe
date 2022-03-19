<template>
  <div class="container">
    <div class="sort">
      <button
        class="cart-btn"
        value="add"
        v-if="cart.length === 0 && showProduct !== false"
        disabled
      >
        Cart&nbsp;&nbsp;{{ cartItemCount }}&nbsp;
        <span class="fas fa-cart-plus"></span>
      </button>
      <button class="cart-btn" value="add" @click="showCheckout" v-else>
        Cart&nbsp;&nbsp;{{ cartItemCount }}&nbsp;
        <span class="fas fa-cart-plus"></span>
      </button>
    </div>
    <div v-if="showProduct">
      <lesson-comp :Lessons="lessons" @lessonId="getLessonInfo" @cart="getCart">
      </lesson-comp>
    </div>
    <div v-else>
      <checkout-comp :cart="cart" :lessonInfo="lessonInfo"></checkout-comp>
    </div>
  </div>
</template>

<script>
import CheckoutComp from "./components/CheckoutComp.vue";
import LessonComp from "./components/LessonComp.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    CheckoutComp: CheckoutComp,
    LessonComp: LessonComp,
  },
  data() {
    return {
      showProduct: true,
      lessons: [],
      users: [],
      lessonInfo: [],
      cart: [],
    };
  },

  created() {
    this.getData();
    this.getUsers();
  },
  computed: {
    cartItemCount: function () {
      return this.cart.length;
    },
  },

  methods: {
    async getData() {
      try {
        const response = await axios.get(
          "https://courseworktwoserver.herokuapp.com/collection/Lessons/"
        );
        // JSON responses are automatically parsed.
        this.lessons = response.data;
        console.log(this.users);
      } catch (error) {
        console.log(error);
      }
    },
    async getUsers() {
      try {
        const response = await axios.get(
          "https://courseworktwoserver.herokuapp.com/collection/Orders/"
        );
        // JSON responses are automatically parsed.
        this.users = response.data;
        console.log(this.users);
      } catch (error) {
        console.log(error);
      }
    },
    showCheckout() {
      this.showProduct = this.showProduct ? false : true;
    },
    getCart(value) {
      this.cart = value;
    },
    getLessonInfo(value) {
      this.lessonInfo = value;
    },
  },
};
</script>

<style>
</style>
