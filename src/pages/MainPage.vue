<template>
  <main class="content container">
    <div class="content__top content__top--catalog">
      <h1 class="content__title">
        Каталог
      </h1>
      <span class="content__info">
        {{ validateProductsCount }}
      </span>
    </div>
    <div class="content__catalog">
      <ProductFilter :price-from.sync="filterPriceFrom" :price-to.sync="filterPriceTo" :category-id.sync="filterCategoryId"/>

      <section class="catalog">
        <div class="" v-if="productsLoading">Загрузка товаров...</div>
        <div class="" v-if="productsLoadingFailed">
          Произошла ошибка при загрузке товаров...
          <button @click.prevent="loadProducts">Попробовать снова</button>
        </div>
        <ProductList :products="products"/>
        <BasePagination v-model="page" :count="countProducts" :per-page="productsPerPage"/>
      </section>
    </div>
  </main>
</template>
<script>
  import ProductList from '@/components/ProductList.vue'
  import BasePagination from '@/components/BasePagination.vue'
  import ProductFilter from '@/components/ProductFilter.vue'
  import axios from 'axios'
  import {API_BASE_URL} from '@/config.js'

  export default {
    components: {ProductList, BasePagination, ProductFilter},
    data() {
      return {
        filterPriceFrom: 0,
        filterPriceTo: 0,
        filterCategoryId: 0,

        page: 1,
        productsPerPage: 3,

        productsData: null,
        productsLoading: false,
        productsLoadingFailed: false
      }
    },
    computed: {
      products() {
        return this.productsData 
        ? this.productsData.items.map(product => {
          return {
            ...product,
            image: product.image.file.url
          }
        })
        : []
      },
      countProducts() {
        return this.productsData ? this.productsData.pagination.total : 0
      },
      validateProductsCount() {
        const amount = String(this.productsData ? this.productsData.pagination.total : 0)
        let name = ''
        if (amount[amount.length - 1] === '1') {
          name = 'товар'
          console.log(amount)
          console.log(name)
        }
        else if (amount[amount.length - 1] === '2' || amount[amount.length - 1] === '3' || amount[amount.length - 1] === '4') {
          name = 'товара'
        }
        else {
          name = 'товаров'
        }

        return amount + ' ' + name
      }
    },
    methods: {
      loadProducts() {
        this.productsLoading = true
        this.productsLoadingFailed = false
        clearTimeout(this.loadProductsTimer)
        this.loadProductsTimer =  setTimeout(() => {
          axios.get(API_BASE_URL + '/api/products', {
            params: {
              page: this.page,
              limit: this.productsPerPage,
              categoryId: this.filterCategoryId,
              minPrice: this.filterPriceFrom,
              maxPrice: this.filterPriceTo
            }
          })
          .then(respsonse => this.productsData = respsonse.data)
          .catch(() => this.productsLoadingFailed = true)
          .then(() => this.productsLoading = false)
        }, 0)
      }
    },
    watch: {
      page() {
        this.loadProducts()
      },
      filterPriceFrom() {
        this.loadProducts()
      },
      filterPriceTo() {
        this.loadProducts()
      },
      filterCategoryId() {
        this.loadProducts()
      }
    },
    created() {
      this.loadProducts()
    }

  }

</script>