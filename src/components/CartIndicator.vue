<template>
  <router-link class="header__cart" aria-label="Корзина с товарами" :to="{ name: 'cart' }">
    <span v-if="cartLoading === true">Загрузка корзины...</span>
    <div v-if="cartLoading === false">
      <svg width="19" height="24">
          <use xlink:href="#icon-cart"></use>
        </svg>
      <span class="header__count" aria-label="Количество товаров">
        {{ $store.state.cartProducts.length }}
      </span>
    </div>
  </router-link>
</template>

<script>
export default {
  props: ['cartLoading'],
};
</script>