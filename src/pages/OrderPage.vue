<template>
  <main class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'main' }">Каталог</router-link>
        </li>
        <li class="breadcrumbs__item">
          <router-link class="breadcrumbs__link" :to="{ name: 'cart' }">Корзина</router-link>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Оформление заказа
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Корзина
      </h1>
      <span class="content__info">
        {{ products.length }} товара
      </span>
    </div>

    <section class="cart">
      <form class="cart__form form" action="#" method="POST" @submit.prevent="order">
        <div class="cart__field">
          <div class="cart__data">
            <BaseFormText title="ФИО" placeholder="Введите ваше полное имя" v-model="formData.name"
              :error="formError.name" type="text" />
            <BaseFormText title="Адрес доставки" placeholder="Введите ваш адрес" v-model="formData.address"
              :error="formError.address" type="text" />
            <BaseFormText title="Телефон" placeholder="Введите ваш телефон" v-model="formData.phone"
              :error="formError.phone" type="text" />
            <BaseFormText title="Email" placeholder="Введи ваш Email" v-model="formData.email" :error="formError.email"
              type="email" />
            <BaseFormTextArea title="Комментарий к заказу" v-model="formData.comment" :error="formError.comment"
              placeholder="Ваши пожелания" />
          </div>

          <div class="cart__options">
            <h3 class="cart__title">Доставка</h3>
            <ul class="cart__options options" v-if="deliveriesData">
              <li class="options__item" v-for="del, index in deliveriesData.length" :key="index">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="delivery" v-model="formData.deliveryTypeId"
                    :value="deliveriesData[index].id" @click="getPayments(deliveriesData[index])">
                  <span class="options__value">
                    {{ deliveriesData[index].title }} ({{ deliveriesData[index].price }} ₽)
                  </span>
                </label>
              </li>
              <span class="error" style="color: red;" v-if="formError.deliveryTypeId">{{ formError.deliveryTypeId }}</span>
            </ul>

            <h3 class="cart__title">Оплата</h3>
            <ul class="cart__options options" v-if="paymentsData">
              <li class="options__item" v-for="del, index in paymentsData.length" :key="index">
                <label class="options__label">
                  <input class="options__radio sr-only" type="radio" name="pay" v-model="formData.paymentTypeId"
                    :value="paymentsData[index].id">
                  <span class="options__value">
                    {{ paymentsData[index].title }}
                  </span>
                </label>
              </li>
              <span class="error" style="color: red;" v-if="formError.paymentTypeId">{{ formError.paymentTypeId }}</span>
            </ul>
          </div>
        </div>

        <div class="cart__block">

          <OrderBlock v-for="item in products" :key="item.productId" :item="item" />

          <div class="cart__total">
            <p v-if="deliveryCount">Доставка: <b>{{ deliveryCount }} ₽</b></p>
            <p>Итого: <b>{{ products.length }}</b> товара на сумму <b>{{ Number(totalPrice) + Number(deliveryCount) | numberFormat }} ₽</b></p>
          </div>

          <button class="cart__button button button--primery" type="submit" :disabled="loadOrder">
            Оформить заказ
          </button>
          <span v-if="loadOrder">Оформление заказа...</span>
        </div>
        <div class="cart__error form__error-block" v-if="formErrorMessage">
          <h4>Заявка не отправлена!</h4>
          <p>
            {{ formErrorMessage }}
          </p>
        </div>
      </form>
    </section>
  </main>
</template>

<script>
import BaseFormText from '@/components/BaseFormText.vue';
import BaseFormTextArea from '@/components/BaseFormTextArea.vue'
import { API_BASE_URL } from '@/config';
import OrderBlock from '@/components/OrderBlock.vue';
import axios from 'axios';
import numberFormat from '@/helpers/numberFormat';
import { mapGetters } from 'vuex';

export default {
  components: { BaseFormText, BaseFormTextArea, OrderBlock },
  data() {
    return {
      formData: {},
      formError: {},
      formErrorMessage: '',
      products: this.$store.state.cartProductsData,
      loadOrder: false,
      deliveriesData: null,
      deliveryCount: '',
      paymentsData: null,
    };
  },
  methods: {
    deliveriesDataLoad() {
      axios.get(API_BASE_URL + '/api/deliveries',)
        .then(response => {
          this.deliveriesData = response.data;
        })
    },
    getPayments(obj) {
      this.deliveryCount = obj.price;
      axios.get(API_BASE_URL + '/api/payments', {
        params: {
          deliveryTypeId: obj.id,
        },
      })
        .then(response => {
          this.paymentsData = response.data;
        })
    },
    paymentId(value) {
      this.formData.paymentTypeId = value;
    },
    order() {
      this.formError = {};
      this.formErrorMessage = '';
      this.loadOrder = true;
      clearTimeout(this.orderTimer);
      this.orderTimer = setTimeout(() => {

        axios.post(API_BASE_URL + '/api/orders', {
          ...this.formData
        }, {
          params: {
            userAccessKey: this.$store.state.userAccessKey
          },
        })
          .then(response => {
            this.$store.commit('resetCart');
            this.$store.commit('updateOrderInfo', response.data);
            this.$router.push({ name: 'orderInfo', params: { id: response.data.id } });
            this.loadOrder = false;
          })
          .catch(error => {
            this.formError = error.response.data.error.request || {}
            this.formErrorMessage = error.response.data.error.message;
            this.loadOrder = false;
          })
      }, 0);
    },
  },
  computed: {
    ...mapGetters({ totalPrice: 'cartTotalPrice' }),
  },
  filters: {
    numberFormat,
  },
  created() {
    this.deliveriesDataLoad();
  },
};

</script>