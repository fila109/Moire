<template>
  <li class="cart__item product">
    <div class="product__pic">
      <img v-if="currentColor.gallery" :src="currentColor.gallery[0].file.url" width="120" height="120" :alt="item.product.title">
      <span v-else>Нет изображения</span>
    </div>
    <h3 class="product__title">
      {{ item.product.title }}
    </h3>
    <p class="product__info product__info--color">
      <span>
        Цвет:
        <i :style="{ 'backgroundColor': `${currentColor.color.code}` }"></i>
        {{ currentColor.color.title }}
      </span>
      <span>
        Размер:
        {{ item.size.title }}
      </span>
    </p>
    <span class="product__code">
      Артикул: {{ item.productId }}
    </span>

    <div class="product__counter form__counter">
      <button type="button" aria-label="Убрать один товар" @click="decrementCart">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-minus"></use>
        </svg>
      </button>

      <input v-model.number="quantity" type="number" name="count">

      <button type="button" aria-label="Добавить один товар" @click="incrementCart">
        <svg width="10" height="10" fill="currentColor">
          <use xlink:href="#icon-plus"></use>
        </svg>
      </button>
    </div>

    <b class="product__price">
      {{ item.quantity * item.product.price | numberFormat }} ₽
    </b>

    <button class="product__del button-del" type="button" aria-label="Удалить товар из корзины" @click.prevent="deleteProduct(item.productId)">
      <svg width="20" height="20" fill="currentColor">
        <use xlink:href="#icon-close"></use>
      </svg>
    </button>
  </li>
</template>

<script>

import numberFormat from '@/helpers/numberFormat';
import { mapActions } from 'vuex';

export default {
  filters: {
    numberFormat,
  },
  props: ['item'],
  computed: {
    currentColor() {
      return this.item.product.colors.find(col => col.color.id === this.item.color);
    },
    quantity: {
      get() {
        return this.item.quantity;
      },
      set(value) {
        this.$store.dispatch('updateCartProductAmount', { productId: this.item.productId, quantity: value });
      },
    },
  },
  methods: {
    ...mapActions({ deleteProduct: 'deleteCartProduct' }),
    incrementCart() {
      this.quantity++;
    },
    decrementCart() {
      if (this.quantity > 1) {
        this.quantity--;
      }
    }
  },
  
};

</script>