<template>
  <div>
    <router-link class="catalog__pic" :to="{ name: 'product', params: { id: product.id } }">
      <img v-if="product.colors[currentProductColor].gallery" :src=product.colors[currentProductColor].gallery[0].file.url>
      <span v-else>Нет изображения</span>
    </router-link>

    <h3 class="catalog__title">
      <a href="#">
        {{ product.title }}
      </a>
    </h3>

    <span class="catalog__price">
      {{ product.price | numberFormat }} Р
    </span>

    <ul class="colors colors--black">
      <li class="colors__item" v-for="(col, index) in product.colors.length" :key="col.id">
        <label class="colors__label">
          <input class="colors__radio sr-only" type="radio" v-model="currentProductColor"
            :value="index">
          <span class="colors__value" :style="{ 'backgroundColor': `${product.colors[index].color.code}` }">
          </span>
        </label>
      </li>
    </ul>
  </div>
</template>
<script>
import numberFormat from '@/helpers/numberFormat';

export default {
  data() {
    return {
      currentProductColor: 0,
    };
  },
  props: ['product'],
  filters: {
    numberFormat,
  },
};
</script>