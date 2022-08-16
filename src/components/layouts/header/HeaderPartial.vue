<template>
  <nav class="navbar navbar-expand navbar-light bg-light sticky-top">
    <div class="container">
      <router-link
        to="/"
        class="navbar-brand"
      >
      Inventory Applications
      </router-link>
      <div class="navbar-nav mr-auto">
        <li class="nav-item" v-if="user.data">
          <router-link
            to="/products"
            class="nav-link"
          >
            <i class="fa fa-shopping-cart" />
            Products
          </router-link>
        </li>
        <li class="nav-item" v-if="user.data">
          <router-link
            to="/products/create"
            class="nav-link"
          >
            <i class="fa fa-plus-circle" /> Add Product
          </router-link>
        </li>

        <li class="nav-item" v-if="user.data">
          <button
              class="btn btn-danger btn-block"
              @click.prevent="logout"
          >
            Log Out
          </button>
        </li>

      </div>
    </div>
  </nav>
</template>

<script>
import {mapActions, mapGetters} from "vuex";

export default {
  name: "HeaderPartial",
  data() {
    return {
      activeMenu:localStorage.getItem('accessToken'),
    };
  },
  computed: { ...mapGetters(["user"]) },
  methods: {
    ...mapActions([
      "updateState",
    ]),
    logout() {
      this.updateState();
      this.$router.push({ name: "Login" });
    },
  },
};
</script>
