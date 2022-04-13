<script>
export default {
  props: {
    productList: {
      type: Array,
      required: true,
    },
  },
}
</script>

<template>
  <div class="product-grid">
    <product-card
      v-for="product in productList"
      :key="product.node.id"
      :product="product.node"
    />
  </div>
</template>
