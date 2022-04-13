<script>
import { mapGetters } from 'vuex'

export default {
  computed: {
    ...mapGetters({
      cartSize: 'cart/size',
    }),
  },
}
</script>

<template>
  <header class="app-header">
    <h1>Shoperoni</h1>
    <nav class="main-nav">
      <ul>
        <li class="main-nav-item">
          <div v-if="cartSize > 0" class="cart-size">{{ cartSize }}</div>
          <nuxt-link to="/cart">Cart</nuxt-link>
        </li>
      </ul>
    </nav>
  </header>
</template>
