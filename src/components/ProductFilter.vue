<template>
  <aside class="filter">

    <form class="filter__form form" action="#" method="get" @submit.prevent="submit">
      <fieldset class="form__block">
        <legend class="form__legend">Цена</legend>
        <label class="form__label form__label--price" for="name">
          <input class="form__input" type="text" name="min-price" v-model.number="currentPriceFrom">
          <span class="form__value">От</span>
        </label>
        <label class="form__label form__label--price" for="name">
          <input class="form__input" type="text" name="max-price" v-model.number="currentPriceTo">
          <span class="form__value">До</span>
        </label>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Категория</legend>
        <label class="form__label form__label--select" for="name">
          <select class="form__select" type="text" name="category" v-model.number="currentCategoryId">
            <option v-if="categoryId===0" value="0">Все категории</option>
            <option v-if="categoryId>0 && categories[categoryId-1]" value="0">{{ categories[categoryId-1].title }}</option>
            <option :value="category.id" v-for="category in categories" :key="category.id">{{ category.title }}</option>
          </select>
        </label>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Материал</legend>
        <ul class="check-list">
          <li class="check-list__item" v-for="(material, index) in materials" :key="material.id">
            <label class="check-list__label">
              <input class="check-list__check sr-only" type="checkbox" name="material" :value="material.id" v-model="currentMaterialIds">
              <span class="check-list__desc">
               {{ material.title }}
                <span>({{ materials[index].productsCount }})</span>
              </span>
            </label>
          </li>
        </ul>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Коллекция</legend>
        <ul class="check-list">
          <li class="check-list__item"  v-for="(season, index) in seasons" :key="season.id">
            <label class="check-list__label">
              <input class="check-list__check sr-only" type="checkbox" name="collection" :value="season.id" v-model="currentSeasonIds">
              <span class="check-list__desc">
                {{ season.title }}
                <span>({{ seasons[index].productsCount }})</span>
              </span>
            </label>
          </li>
        </ul>
      </fieldset>

      <button class="filter__submit button button--primery" type="submit" @click.prevent="submit">
        Применить
      </button>
      <button class="filter__reset button button--second" type="button" @click.prevent="reset" v-if="!showReset">
        Сбросить
      </button>
    </form>
  </aside>
</template>

<script>
import axios from 'axios';
import { API_BASE_URL } from '@/config';

export default {
  data() {
    return {
      currentPriceFrom: 0,
      currentPriceTo: 0,
      currentCategoryId: 0,
      currentColorIds: [],
      currentMaterialIds: [],
      currentSeasonIds: [],
      categoriesData: null,
      materialsData: null,
      seasonsData: null,
      colorsData: null,
    };
  },

  props: ['priceFrom', 'priceTo', 'categoryId', 'colorIds', 'materialIds', 'seasonIds', 'showReset'],
  computed: {
    categories() {
      return this.categoriesData ? this.categoriesData.items : [];
    },
    colors() {
      return this.colorsData ? this.colorsData.items : [];
    },
    materials() {
      return this.materialsData ? this.materialsData.items : [];
    },
    seasons() {
      return this.seasonsData ? this.seasonsData.items : [];
    },
  },
  watch: {
    priceFrom(value) {
      this.currentPriceFrom = value;
    },
    priceTo(value) {
      this.currentPriceTo = value;
    },
    categoryId(value) {
      this.currentCategoryId = value;
    },
    colorIds(value) {
      this.currentColorIds = value;
    },
    materialIds(value) {
      this.currentMaterialIds = value;
    },
    seasonIds(value) {
      this.currentSeasonIds = value;
    },
  },
  methods: {
    submit() {
      this.$emit('update:priceFrom', this.currentPriceFrom);
      this.$emit('update:priceTo', this.currentPriceTo);
      this.$emit('update:categoryId', this.currentCategoryId);
      this.$emit('update:colorIds', this.currentColorIds);
      this.$emit('update:materialIds', this.currentMaterialIds);
      this.$emit('update:seasonIds', this.currentSeasonIds);
      this.$emit('update:showReset', false);
    },
    reset() {
      this.$emit('update:priceFrom', 0);
      this.$emit('update:priceTo', 0);
      this.$emit('update:categoryId', 0);
      this.$emit('update:colorIds', []);
      this.$emit('update:materialIds', []);
      this.$emit('update:seasonIds', []);
      this.$emit('update:showReset', true);
      this.$router.push({ name: 'main' }).catch(()=>{});
    },
    loadCategorirs() {
      axios.get(API_BASE_URL + '/api/productCategories')
        .then(response => this.categoriesData = response.data)
    },
    loadColors() {
      axios.get(API_BASE_URL + '/api/colors')
        .then(response => this.colorsData = response.data)
    },
    loadMaterials() {
      axios.get(API_BASE_URL + '/api/materials')
        .then(response => this.materialsData = response.data)
    },
    loadSeasons() {
      axios.get(API_BASE_URL + '/api/seasons')
        .then(response => this.seasonsData = response.data)
    },
  },
  created() {
    this.loadCategorirs();
    this.loadColors();
    this.loadMaterials();
    this.loadSeasons();
  }
};
</script>