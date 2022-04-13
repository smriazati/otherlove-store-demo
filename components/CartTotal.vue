<script>
import { mapGetters } from 'vuex'

export default {
  computed: {
    ...mapGetters({
      subtotal: 'cart/subtotal',
      tax: 'cart/tax',
      total: 'cart/total',
    }),
  },
}
</script>

<template>
  <section class="cart-total">
    <div class="cart-total-content">
      <div class="cart-total-column">
        <p>
          <strong>Subtotal:</strong>
        </p>
        <p>Shipping:</p>
        <p>Tax:</p>
        <p>Total:</p>
      </div>
      <div class="cart-total-column">
        <p>
          <strong>{{ subtotal }}</strong>
        </p>
        <p>Free Shipping</p>
        <p>{{ tax }}</p>
        <p>{{ total }}</p>
      </div>
    </div>
  </section>
</template>
