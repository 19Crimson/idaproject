<template>
  <div class="product-card">
    <div class="delete-wrap" @click="() => onDelete(data.id)">
      <div class="delete-icon" />
    </div>
    <img :src="data.image" :alt="data.name" :class="{ 'product-image': true, preload: loading }">
    <div class="product-info">
      <p class="product-name">
        {{ data.name }}
      </p>
      <p class="product-description">
        {{ data.description }}
      </p>
      <p class="product-price">
        {{ price }} руб.
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      default () { }
    },
    onDelete: {
      type: Function,
      default () {
        return { }
      }
    }
  },
  data () {
    return {
      loading: true
    }
  },
  computed: {
    price () {
      return this.data.price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ')
    }
  },
  mounted () {
    const image = new Image()
    image.onload = () => {
      this.loading = false
    }
    image.src = this.data.image
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/css/mixins.scss";

.product-card {
  transition: all 1s;
  background: #fffefb;
  border-radius: 4px;
  cursor: pointer;
  position: relative;
  @include box-shadow(0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02));

  &:hover {
    @include box-shadow(7px 8px 10px 0px rgba(34, 60, 80, 0.2));

    .delete-wrap {
      display: block;
    }
  }
}

.delete-wrap {
  display: none;
  position: absolute;
  width: 32px;
  height: 32px;
  background: #ff8484;
  @include box-shadow(0px 2px 4px rgba(0, 0, 0, 0.1));
  border-radius: 10px;
  padding: 7px 9px;
  right: -8px;
}

.delete-icon {
  width: 13px;
  height: 16px;
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
  background-image: url("~/assets/images/deleteIcon.png");
}

.product-image {
  object-fit: cover;
  width: 100%;
  height: 200px;

  @media (max-width: 1423px) {
    height: 174px;
  }
  @media (max-width: 1284px) {
    height: 149px;
  }

  &.preload {
    content: url('~/assets/images/preloader.gif');
    width: 64px;
    height: 64px;
    margin: 68px 134px;

    @media (max-width: 1423px) {
      margin: 55px 108px;
    }
    @media (max-width: 1284px) {
      margin: 42px 88px;
    }
  }
}

.product-info {
  padding: 12px 16px;
}

.product-name {
  font-weight: 600;
  font-size: 20px;
  color: #3f3f3f;
  margin-bottom: 16px;
  overflow: hidden;
  text-overflow: ellipsis;

  @media (max-width: 1284px) {
    margin-bottom: 8px;
  }
}

.product-description {
  max-height: 80px;
  font-weight: 400;
  font-size: 16px;
  color: #3f3f3f;
  margin-bottom: 32px;
  overflow: hidden;
  text-overflow: ellipsis;

  @media (max-width: 1423px) {
    max-height: 62px;
  }
}

.product-price {
  font-weight: 600;
  font-size: 24px;
  line-height: 30px;
  position:  absolute;
  left: 16px;
  bottom: 24px;
}

@media (max-width: 1423px) {
  .product-price {
    bottom: 15px;
  }
}
</style>
