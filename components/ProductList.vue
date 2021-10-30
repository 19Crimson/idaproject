<template>
  <div class="wrapper">
    <div class="header">
      <h1 class="title">
        Добавление товара
      </h1>
      <select-item :callback="onChangeSort" />
    </div>
    <div class="content">
      <div class="sidebar-wrapper">
        <sidebar :on-create="onCreateProduct" />
      </div>
      <div class="products-wrapper">
        <transition-group
          v-if="products.length"
          name="products-group"
          tag="div"
          class="products"
        >
          <product
            v-for="item in products"
            :key="item.id"
            :data="item"
            :on-delete="onDeleteProduct"
          />
        </transition-group>
        <p v-else class="no-data">
          Не найдено товаров
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import Select from './Select.vue'
import Form from './Form.vue'
import Product from './Product.vue'

export default {
  components: {
    'select-item': Select,
    sidebar: Form,
    product: Product
  },
  data () {
    return {
      products: [],
      loading: true
    }
  },
  mounted () {
    try {
      this.products = JSON.parse(localStorage.getItem('products')) || []
      this.loading = false
    } catch (error) {
      this.products = []
      this.loading = false
    }
  },
  methods: {
    onCreateProduct (product) {
      return new Promise((resolve, reject) => {
        const resultProduct = {
          id: Math.random().toString(36).substr(2, 9),
          name: product.name.value,
          description: product.description.value,
          image: product.imageLink.value,
          price: product.price.value
        }
        this.products.push(resultProduct)
        localStorage.setItem('products', JSON.stringify(this.products))
        resolve()
      })
    },
    onDeleteProduct (id) {
      this.products = this.products.filter(product => product.id !== id)
      localStorage.setItem('products', JSON.stringify(this.products))
    },
    onChangeSort (sortValue) {
      const sortFuncs = {
        name: (a, b) => (a.name > b.name ? 1 : -1),
        maxPrice: (a, b) => Number(b.price) - Number(a.price),
        minPrice: (a, b) => Number(a.price) - Number(b.price)
      }
      this.products = this.products.sort(sortFuncs[sortValue])
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 32px 32px 0px;
  margin: 0 auto;
  max-width: 1440px;
  width: 100%;
}

.header {
  display: flex;
  justify-content: space-between;
}

.title {
  font-weight: 600;
  font-size: 28px;
  line-height: 35px;
  color: #3F3F3F;
}

.content {
  display: flex;
  justify-content: space-between;
  margin-top: 16px;
}

.sidebar-wrapper {
  height: 440px;
  width: 332px;
}

.products-wrapper {
  flex: 1;
  position: relative;
}

.products {
  flex: 1;
  display: grid;
  grid-template-columns: repeat(auto-fit, 332px);
  grid-gap: 16px;
  grid-auto-rows: 423px;
  height: 100%;
  margin-left: 16px;
  max-height: calc(100vh - 100px);
  overflow-y: auto;
  overflow-x: hidden;
  justify-content: center;

  &::-webkit-scrollbar {
    width: 0;
  }

  @media (max-width: 1423px) {
    grid-template-columns: repeat(auto-fit, 280px);
    grid-auto-rows: 368px;
  }

  @media (max-width: 1284px) {
    grid-template-columns: repeat(auto-fit, 240px);
    grid-auto-rows: 316px;
  }
}

.no-data {
  flex: 1;
  text-align: center;
}

.products-group-enter,
.products-group-leave-to {
  opacity: 0;
}

.products-group-leave-active {
  position: absolute;
}
</style>
