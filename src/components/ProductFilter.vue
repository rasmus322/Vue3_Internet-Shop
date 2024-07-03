<template>
  <aside class="filter">
    <h2 class="filter__title">Фильтры</h2>

    <form class="filter__form form" action="#" method="get" @submit.prevent="submit">
      <fieldset class="form__block">
        <legend class="form__legend">Цена</legend>
        <label class="form__label form__label--price" for="min-price">
          <input class="form__input" type="text" name="min-price"
          v-model.number="currentPriceFrom"/>
          <span class="form__value">От</span>
        </label>
        <label class="form__label form__label--price" for="max-price">
          <input class="form__input" type="text" name="max-price"
          v-model.number="currentPriceTo"/>
          <span class="form__value">До</span>
        </label>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Категория</legend>
        <label class="form__label form__label--select" for="category">
          <select class="form__select" type="text" name="category"
          v-model.number="currentCategoryId">
            <option value=0>Все категории</option>
            <option :value="category.id" v-for="category in categories" :key="category.id">
              {{ category.title }}
            </option>
          </select>
        </label>
      </fieldset>

      <button class="filter__submit button button--primery" type="submit">Применить</button>
      <button class="filter__reset button button--second" type="button" @click.prevent="reset">
        Сбросить
      </button>
    </form>
  </aside>
</template>

<script>
import axios from 'axios'
import {API_BASE_URL} from '@/config.js'

export default {
  props: ['priceFrom', 'priceTo', 'categoryId'],
  data() {
    return {
      currentPriceFrom: 0,
      currentPriceTo: 0,
      currentCategoryId: 0,

      categoriesData: null,
      colorsData: null
    }
  },
  computed: {
    categories() {
      return this.categoriesData ? this.categoriesData.items : [];
    }
  },
  watch: {
    priceFrom(value) {
      this.currentPriceFrom = value
    },
    priceTo(value) {
      this.currentPriceTo = value
    },
    categoryId(value) {
      this.currentCategoryId = value
    }
  },
  methods: {
    submit() {
      this.$emit('update:price-from', this.currentPriceFrom);
      this.$emit('update:price-to', this.currentPriceTo);
      this.$emit('update:category-id', this.currentCategoryId);
    },
    reset() {
      this.currentPriceFrom = 0;
      this.currentPriceTo = 0;
      this.currentCategoryId = 0;
      this.submit();
    },
    loadCategories() {
      axios.get(API_BASE_URL + '/api/productCategories')
        .then(response => this.categoriesData = response.data)
    }
  },
  created() {
    this.loadCategories()
  }
};
</script>

<style>

</style>