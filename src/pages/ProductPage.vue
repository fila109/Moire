<template>
  <main class="content container" v-if="productLoading">Загрузка товара...</main>
  <main class="content container" v-else-if="!productData">Не удалось загрузить товар</main>
  <main class="content container" v-else>
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'main' }">
            Каталог
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link"
            :to="{ name: 'main', query: { filterCategoryId: this.productData.category.id } }">
            {{ category.title }}
          </router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            {{ product.title }}
          </a>
        </li>
      </ul>
    </div>

    <section class="item">
      <div class="item__pics pics">
        <div class="pics__wrapper" v-if="product.colors[currentColor].gallery">
          <img width="570" height="570" :src="product.colors[currentColor].gallery[0].file.url" :alt="product.title">
          <div class="pics__item" v-for="image,index in product.colors.length" :key="index">
            <div class="pics__link pics__link--current" v-if="product.colors[index].gallery">
              <img :src="product.colors[index].gallery[0].file.url" :alt="product.title"
                @click="changeColor(index, product.colors[index].color.id)">
            </div>
          </div>
        </div>
      </div>

      <div class="item__info">
        <span class="item__code">Артикул: {{ product.id }}</span>
        <h2 class="item__title">
          {{ product.title }}
        </h2>
        <div class="item__form">
          <form class="form" action="#" method="POST" @submit.prevent="addToCart">
            <b class="item__price">
              {{ product.price | numberFormat }} ₽
            </b>

            <fieldset class="form__block">
              <legend class="form__legend">Цвет:</legend>
              <ul class="colors">
                <li class="colors__item" v-for="(color, index) in product.colors.length" :key="color.id">
                  <label class="colors__label">
                    <input class="colors__radio sr-only" type="radio" name="color-item" :value="product.colors[index].color.id"
                      v-model="productColor" @click="changeColor(index)">
                    <span class="colors__value" :style="{ 'backgroundColor': `${product.colors[index].color.code}` }">
                    </span>
                  </label>
                </li>
              </ul>
            </fieldset>

            <fieldset class="form__block">
              <legend class="form__legend">Размер:</legend>
              <label class="form__label form__label--small form__label--select">
                  <select class="form__select" type="text" name="category" v-model="productSize">
                    <option v-for="(size, index) in product.sizes.length" :key="size.id" :value="product.sizes[index]">
                      {{ product.sizes[index].title }}</option>
                  </select>
                </label>
            </fieldset>

            <div class="item__row">
              <div class="form__counter">
                <button type="button" aria-label="Убрать один товар" @click="decrementProduct">
                  <svg width="12" height="12" fill="currentColor">
                    <use xlink:href="#icon-minus"></use>
                  </svg>
                </button>

                <input type="text" v-model.number="productAmount">

                <button type="button" aria-label="Добавить один товар" @click="incrementProduct">
                  <svg width="12" height="12" fill="currentColor">
                    <use xlink:href="#icon-plus"></use>
                  </svg>
                </button>
              </div>

              <button class="item__button button button--primery" type="submit"
                :disabled="productAddSending || productAmount < 1">
                В корзину
              </button>
            </div>
            
            <div style="margin-top: 20px;" v-show="productAdded">Товар добавлен в корзину</div>
            <div v-show="productAddSending" style="display: flex; align-items: center;">
              <div style="display: block; width: 100px; height: 100px;">
                <img style="width: 100%; height: 100%;" src="img/animation/loading-6.gif" alt="Animation">
              </div>
              <p>Добавляем товар в корзину...</p>
            </div>
          </form>
        </div>
      </div>

      <div class="item__desc">
        <ul class="tabs">
          <li class="tabs__item">
            <a href="#" class="tabs__link" :class="{ 'tabs__link--current': currentTab === 1 }"
              @click.prevent="moveTab(1)">
              Информация о товаре
            </a>
          </li>
          <li class="tabs__item">
            <a href="#" class="tabs__link" :class="{ 'tabs__link--current': currentTab === 2 }"
              @click.prevent="moveTab(2)">
              Доставка и возврат
            </a>
          </li>
        </ul>

        <div class="item__content" v-if="currentTab === 1">
          <h3>Состав:</h3>

          <p>
            60% хлопок<br>
            30% полиэстер<br>
          </p>

          <h3>Уход:</h3>

          <p>
            Машинная стирка при макс. 30ºC короткий отжим<br>
            Гладить при макс. 110ºC<br>
            Не использовать машинную сушку<br>
            Отбеливать запрещено<br>
            Не подвергать химчистке<br>
          </p>

        </div>

        <div class="item__content" v-if="currentTab === 2">
          <h3>Доставка:</h3>
          <p>
            1. Курьерская доставка по Москве и Московской области – 1200 ₽
            2.Самовывоз из магазина. Список и адреса магазинов Вы можете посмотреть <a href="#">здесь</a>
          </p>
          <h3>Возврат:</h3>
          <p>
            Любой возврат должен быть осуществлен в течение 30 дней с момента отгрузки.
            Возвраты в магазине БЕСПЛАТНО.
            Вы можете вернуть товары в любой магазин. Магазин должен быть расположен в стране, в которой Вы осуществили
            покупку.
            БЕСПЛАТНЫЙ возврат в Пункт выдачи заказов.
            Для того чтобы вернуть товар в одном из наших Пунктов выдачи заказов, позвоните по телефону 8 800 600 90 09
          </p>

        </div>
      </div>

    </section>
  </main>
</template>

<script>

import gotoPage from '@/helpers/gotoPage';
import numberFormat from '@/helpers/numberFormat';
import axios from 'axios';
import { API_BASE_URL } from '@/config';
import { mapActions } from 'vuex';

export default {
  model: {
    prop: 'number',
    event: 'moveTab',
  },
  data() {
    return {
      productAmount: 1,
      productData: null,
      productLoading: false,
      productLoadingFailed: false,
      productAdded: false,
      productAddSending: false,
      productSize: null,
      productColor: 0,
      currentTab: 1,
      currentColor: 0,
    };
  },

  filters: {
    numberFormat
  },
  computed: {
    product() {
      return this.productData;
    },
    category() {
      return this.productData.category;
    },
  },
  methods: {
    changeColor(currentColor, productColor) {
      this.currentColor = currentColor;
      this.productColor = productColor;
    },
    moveTab(number) {
      this.currentTab = number;
    },
    ...mapActions(['addProductToCart']),
    gotoPage,
    addToCart() {
      this.productAdded = false;
      this.productAddSending = true;

      this.addProductToCart({ productId: this.product.id, colorId: this.productColor, sizeId: this.productSize.id, quantity: this.productAmount })
        .then(() => {
          this.productAdded = true;
          this.productAddSending = false;
        });
    },
    incrementProduct() {
      this.productAmount++;
    },
    decrementProduct() {
      if (this.productAmount > 1) {
        this.productAmount--;
      }
    },
    loadProduct() {
      this.productLoading = true;
      this.productLoadingFailed = false;
      axios.get(API_BASE_URL + '/api/products/' + this.$route.params.id)
        .then(response => this.productData = response.data)
        .then(() => this.productColor = this.productData.colors[0].color.id)
        .then(() => this.productSize = this.productData.sizes[0])
        .catch(() => this.productLoadingFailed = true)
        .then(() => this.productLoading = false);
    },
  },
  created() {
    this.loadProduct()
  },
  watch: {
    '$route.params.id'() {
      this.loadProduct()
    }
  }
}

</script>
