<script>
import { mapGetters } from 'vuex'

export default {
  async asyncData({ $http, route }) {
    const productData = await $http.$post('/api/get-product', {
      itemHandle: route.params.handle,
    })

    return {
      product: productData.productByHandle,
    }
  },
  data: () => ({
    selectedProductId: '',
    selectedProductQty: 1,
  }),
  head: () => ({
    title: 'Shoperoni - Product',
  }),
  computed: {
    ...mapGetters({
      cartId: 'cart/id',
    }),
    featuredImage() {
      return this.product.images.edges[0].node
    },
    maxQuantity() {
      if (this.selectedProduct) {
        return this.selectedProduct.node.quantityAvailable
      } else {
        return 0
      }
    },
    productVariants() {
      return this.product.variants.edges
    },
    selectedProduct() {
      return this.product.variants.edges.find((item) => {
        return item.node.id === this.selectedProductId
      })
    },
  },
  mounted() {
    // Set default selected item
    this.selectedProductId = this.productVariants[0].node.id

    // Get local cart
    const localCart = window.localStorage.getItem('shopifyNuxtCart')

    if (localCart) {
      this.$store.dispatch('cart/updateBase', JSON.parse(localCart))
    }
  },
  methods: {
    async addToCart() {
      const cartResponse = await this.$http.$post('/api/add-to-cart', {
        cartId: this.cartId,
        itemId: this.selectedProductId,
        quantity: this.selectedProductQty,
      })

      this.$store.dispatch('cart/updateBase', cartResponse)
      window.localStorage.setItem('shopifyNuxtCartId', this.cartId)
    },
    currency(price) {
      const amount = Number(price.amount).toFixed(2)

      return '$' + amount + ` ${price.currencyCode}`
    },
  },
}
</script>

<template>
  <main class="product-page">
    <article>
      <section class="product-page-content">
        <div>
          <img
            class="product-page-image"
            :src="featuredImage.src"
            :alt="featuredImage.altText"
          />
        </div>
        <div>
          <h1>{{ product.title }}</h1>
          <p>{{ product.description }}</p>
          <form method="POST" @submit.prevent="addToCart">
            <div
              v-if="productVariants.length > 1"
              class="product-page-price-list"
            >
              <div
                v-for="{ node: variant } in productVariants"
                :key="variant.id"
                class="product-page-price"
              >
                <input
                  :id="variant.id"
                  v-model="selectedProductId"
                  type="radio"
                  name="merchandiseId"
                  :value="variant.id"
                  :disabled="variant.quantityAvailable === 0"
                />
                <label :for="variant.id">
                  {{ variant.title }} - {{ currency(variant.priceV2) }}
                  <span v-if="variant.quantityAvailable > 10">(10+ left)</span>
                  <span v-else-if="variant.quantityAvailable > 0">
                    (Only {{ variant.quantityAvailable }} left)
                  </span>
                  <span v-else> (Bummer. It's sold out!) </span>
                </label>
              </div>
            </div>
            <div v-else class="product-page-price is-solo">
              {{ currency(productVariants[0].node.priceV2) }}
              <span v-if="productVariants[0].node.quantityAvailable > 10">
                (10+ left)
              </span>
              <span v-else-if="productVariants[0].node.quantityAvailable > 0">
                (Only {{ productVariants[0].node.quantityAvailable }} left)
              </span>
              <span v-else> (Bummer. It's sold out!) </span>
            </div>
            <div class="product-page-quantity-row">
              <input
                v-model.number="selectedProductQty"
                class="product-page-quantity-input"
                type="number"
                name="quantity"
                min="0"
                :max="maxQuantity"
              />

              <button type="submit" class="button purchase">Add to Cart</button>
            </div>
          </form>
        </div>
      </section>
    </article>
  </main>
</template>
