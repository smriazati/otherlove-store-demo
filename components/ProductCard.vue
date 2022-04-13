<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
  computed: {
    featuredImage() {
      if (this.product.images.length > 0) {
        return this.product.images.edges[0].node
      } else {
        return false
      }
    },
    shortDescription() {
      return this.product.description.substring(0, 60) + '...'
    },
  },
}
</script>

<template>
  <div class="product-card">
    <div class="product-card-frame">
      <NuxtLink :to="`/products/${product.handle}`" v-if="featuredImage">
        <img
          class="prodimg"
          :src="featuredImage.src"
          :alt="featuredImage.altText"
        />
      </NuxtLink>
    </div>
    <div class="product-card-text">
      <h3 class="product-card-title">{{ product.title }}</h3>
      <p class="product-card-description">{{ shortDescription }}</p>
    </div>
    <NuxtLink :to="`/products/${product.handle}`">
      <button>View Item ></button>
    </NuxtLink>
  </div>
</template>
