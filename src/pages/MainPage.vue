<template>
  <main class="content container">
    <div class="content__top">
      <div class="content__row">
        <h1 class="content__title">Каталог</h1>
        <span class="content__info"> {{ countProducts }} товара </span>
      </div>
    </div>

    <div class="content__catalog">

      <ProductFilter :price-from.sync="filterPriceFrom" :price-to.sync="filterPriceTo"
        :category-id.sync="filterCategoryId" :color-ids.sync="filterColorIds" :material-ids.sync="filterMaterialIds"
        :season-ids.sync="filterSeasonIds" :show-reset.sync="showReset" />

      <section class="catalog">

        <div style="display: flex; flex-direction: column; align-items: center; margin: 0 auto; " v-if="productsLoading">
          <div style="display: block; width: 200px; height: 200px;">
            <img style="width: 100%; height: 100%;" src="img/animation/loading-6.gif" alt="Animation">
          </div>
          <p>Загрузка товаров...</p>
        </div>
        <div v-if="productsLoadingFailed">Произошла ошибка при загрузка товаров
          <button @click.prevent="loadProducts">Повторить запрос</button>
        </div>

        <ProductList :products="products" />

        <BasePagination :page.sync="page" :count="countProducts" :per-page="productsPerPage" />

<!--        <ul class="catalog__list">
          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-1.jpg" srcset="img/product-1@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Кружевной бюстгалтер без косточек </a>
            </h3>

            <span class="catalog__price"> 3 690 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-1" value="#73B6EA" checked="" />
                  <span class="colors__value" style="background-color: #73b6ea">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-1" value="#8BE000" />
                  <span class="colors__value" style="background-color: #8be000">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-1" value="#222" />
                  <span class="colors__value" style="background-color: #222">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-2.jpg" srcset="img/product-2@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Трусы с кружевными элементами </a>
            </h3>

            <span class="catalog__price"> 5 660 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-2" value="#F0F0F0" checked="" />
                  <span class="colors__value" style="background-color: #f0f0f0">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-2" value="#8BE000" />
                  <span class="colors__value" style="background-color: #8be000">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-2" value="#73B6EA" />
                  <span class="colors__value" style="background-color: #73b6ea">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-3.jpg" srcset="img/product-2@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Теплые шерстяные носки </a>
            </h3>

            <span class="catalog__price"> 790 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-02" value="#F0F0F0" checked="" />
                  <span class="colors__value" style="background-color: #f0f0f0">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-02" value="#8BE000" />
                  <span class="colors__value" style="background-color: #8be000">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-02" value="#73B6EA" />
                  <span class="colors__value" style="background-color: #73b6ea">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-4.jpg" srcset="img/product-4@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Теплые шерстяные носки </a>
            </h3>

            <span class="catalog__price"> 690 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-3" value="#939393" checked="" />
                  <span class="colors__value" style="background-color: #939393">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-3" value="#FF6B00" />
                  <span class="colors__value" style="background-color: #ff6b00">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-3" value="#222" />
                  <span class="colors__value" style="background-color: #222">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-5.jpg" srcset="img/product-5@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Носки с принтом мороженое </a>
            </h3>

            <span class="catalog__price"> 960 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-03" value="#939393" checked="" />
                  <span class="colors__value" style="background-color: #939393">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-03" value="#FF6B00" />
                  <span class="colors__value" style="background-color: #ff6b00">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-03" value="#222" />
                  <span class="colors__value" style="background-color: #222">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-6.jpg" srcset="img/product-7@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Базовая футболка </a>
            </h3>

            <span class="catalog__price"> 960 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-04" value="#FFBE15" checked="" />
                  <span class="colors__value" style="background-color: #ffbe15">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-04" value="#8BE000" />
                  <span class="colors__value" style="background-color: #8be000">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-04" value="#FF6B00" />
                  <span class="colors__value" style="background-color: #ff6b00">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-7.jpg" srcset="img/product-7@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Базовая хлопковая футболка </a>
            </h3>

            <span class="catalog__price"> 900 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-4" value="#FFBE15" checked="" />
                  <span class="colors__value" style="background-color: #ffbe15">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-4" value="#8BE000" />
                  <span class="colors__value" style="background-color: #8be000">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-4" value="#FF6B00" />
                  <span class="colors__value" style="background-color: #ff6b00">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-8.jpg" srcset="img/product-8@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Базовая футболка для сна </a>
            </h3>

            <span class="catalog__price"> 600 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-5" value="#939393" checked="" />
                  <span class="colors__value" style="background-color: #939393">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-5" value="#F0F0F0" />
                  <span class="colors__value" style="background-color: #f0f0f0">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-9.jpg" srcset="img/product-9@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Кружевной бюстгалтер на бретелях </a>
            </h3>

            <span class="catalog__price"> 4 990 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-6" value="#222" checked="" />
                  <span class="colors__value" style="background-color: #222">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-6" value="#73B6EA" />
                  <span class="colors__value" style="background-color: #73b6ea">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-10.jpg" srcset="img/product-10@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Мужские домашние шорты для сна </a>
            </h3>

            <span class="catalog__price"> 1 090 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-7" value="#FFBE15" checked="" />
                  <span class="colors__value" style="background-color: #ffbe15">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-7" value="#8BE000" />
                  <span class="colors__value" style="background-color: #8be000">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-7" value="#222" />
                  <span class="colors__value" style="background-color: #222">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-11.jpg" srcset="img/product-11@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Базовая хлопковая майка </a>
            </h3>

            <span class="catalog__price"> 560 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-07" value="#FFBE15" checked="" />
                  <span class="colors__value" style="background-color: #ffbe15">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-07" value="#222" />
                  <span class="colors__value" style="background-color: #222">
                  </span>
                </label>
              </li>
            </ul>
          </li>

          <li class="catalog__item">
            <a class="catalog__pic" href="#">
              <img src="img/product-12.jpg" srcset="img/product-12@2x.jpg 2x" alt="Название товара" />
            </a>

            <h3 class="catalog__title">
              <a href="#"> Детский комбинезон (1-4 мес) </a>
            </h3>

            <span class="catalog__price"> 990 ₽ </span>

            <ul class="colors colors--black">
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-6" value="#222" checked="" />
                  <span class="colors__value" style="background-color: #222">
                  </span>
                </label>
              </li>
              <li class="colors__item">
                <label class="colors__label">
                  <input class="colors__radio sr-only" type="radio" name="color-6" value="#FF6B00" />
                  <span class="colors__value" style="background-color: #ff6b00">
                  </span>
                </label>
              </li>
            </ul>
          </li>
        </ul>-->

        <!--<ul class="catalog__pagination pagination">
          <li class="pagination__item">
            <a class="pagination__link pagination__link--arrow pagination__link--disabled"
              aria-label="Предыдущая страница">
              <svg width="8" height="14" fill="currentColor">
                <use xlink:href="#icon-arrow-left"></use>
              </svg>
            </a>
          </li>
          <li class="pagination__item">
            <a class="pagination__link pagination__link--current"> 1 </a>
          </li>
          <li class="pagination__item">
            <a class="pagination__link" href="#"> 2 </a>
          </li>
          <li class="pagination__item">
            <a class="pagination__link" href="#"> 3 </a>
          </li>
          <li class="pagination__item">
            <a class="pagination__link" href="#"> 4 </a>
          </li>
          <li class="pagination__item">
            <a class="pagination__link" href="#"> ... </a>
          </li>
          <li class="pagination__item">
            <a class="pagination__link" href="#"> 10 </a>
          </li>
          <li class="pagination__item">
            <a class="pagination__link pagination__link--arrow" href="#" aria-label="Следующая страница">
              <svg width="8" height="14" fill="currentColor">
                <use xlink:href="#icon-arrow-right"></use>
              </svg>
            </a>
          </li>
        </ul>-->
      </section>
    </div>
  </main>
</template>

<script>

import ProductList from '@/components/ProductList.vue';
import BasePagination from '@/components/BasePagination.vue';
import ProductFilter from '@/components/ProductFilter.vue';
import axios from 'axios';
import {API_BASE_URL} from '@/config';

export default {
  components: { ProductList, BasePagination, ProductFilter },
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,
      filterCategoryId: 0,
      filterColorIds: 0,
      filterMaterialIds: [],
      filterSeasonIds: [],
      page: 1,
      productsPerPage: 12,
      productsData: null,
      //colorsData: null,
      productsLoading: false,
      productsLoadingFailed: false,
      showReset: true,
    };
  },
  computed: {
    products() {
      return this.productsData ? this.productsData.items.map(product => {
        return {
          ...product,
          //image: product.image.file.url,
        }
      }) : [];
    },
    countProducts() {
      return this.productsData ? this.productsData.pagination.total : 0;
    },
  },
  methods: {

    loadProducts() {
      this.productsLoading = true,
      this.productsLoadingFailed = false,
      clearTimeout(this.loadProductsTimer);
      this.loadProductsTimer = setTimeout(() => {
        axios.get(API_BASE_URL + `/api/products`, {
        params: {
          page: this.page,
          limit: this.productsPerPage,
          categoryId: this.filterCategoryId,
          minPrice: this.filterPriceFrom,
          maxPrice: this.filterPriceTo,
          colorIds: this.filterColorIds,
          materialIds: this.filterMaterialIds,
          seasonIds: this.filterSeasonIds,
        }
      })
          .then(response => this.productsData = response.data)
          .catch(() => this.productsLoadingFailed = true)
          .then(() => this.productsLoading = false);

      }, 0);
    },
  },
  watch: {
    page() {
      this.loadProducts();
    },
    filterPriceFrom() {
      this.loadProducts();
    },
    filterPriceTo() {
      this.loadProducts();
    },
    filterCategotyId() {
      this.loadProducts();
    },
    filterColorIds() {
      this.loadProducts();
    },
    filterMaterialIds() {
      this.loadProducts();
    },
    filterSeasonIds() {
      this.loadProducts();
    },
  },
  created() {
    if (this.$route.query.filterCategoryId) {
      this.filterCategoryId = this.$route.query.filterCategoryId;
      this.showReset=false;
    }
    this.loadProducts();
  },
};
</script>
