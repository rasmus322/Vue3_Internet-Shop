<template>
  <li class="catalog__item">
    <router-link class="catalog__pic" :to="{name: 'product', params: {id: product.id}}">
      <img :src="product.image" :alt="product.title">
    </router-link>
    <h3 class="catalog__title">
      <a href="#">
        {{ product.title }}
      </a>
    </h3>
    <span class="catalog__price">
      {{ product.price | numberFormat }} р.
    </span>
  </li>
</template>

<script>
  import gotoPage from '@/helpers/gotoPage'
  import numberFormat from '@/helpers/numberFormat';

  export default {
    data() {
      return {};
    },
    filters: {
      numberFormat
    },
    methods: {
      gotoPage
    },
    props: ['product'],
  }
</script>
