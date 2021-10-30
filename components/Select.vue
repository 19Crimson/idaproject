<template>
  <div class="select-wrapper">
    <button class="select-button" @click="onSelect">
      {{ value || "По умолчанию" }}
    </button>
    <div v-if="isOpen" class="select-options-list">
      <p
        v-for="item in sortOptions"
        :key="item.value"
        @click="onClickOption($event, item)"
      >
        {{ item.text }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    callback: {
      type: Function,
      default () {
        return { }
      }
    }
  },
  data () {
    return {
      isOpen: false,
      value: 'По умолчанию',
      sortOptions: [{
        value: 'name',
        text: 'По наименованию'
      }, {
        value: 'maxPrice',
        text: 'По убыванию цены'
      }, {
        value: 'minPrice',
        text: 'По возрастанию цены'
      }]
    }
  },
  methods: {
    onClickOption (e, item) {
      this.isOpen = false
      this.value = item.text || ''
      this.callback(item.value)
    },
    onSelect (e) {
      this.isOpen = !this.isOpen
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/css/mixins.scss";

.select-wrapper {
  position: relative;
}
.select-button {
  display: flex;
  padding: 10px 16px;
  background: #fffefb;
  @include box-shadow(0px 2px 5px rgba(0, 0, 0, 0.1));
  border-radius: 4px;
  font-weight: 400;
  font-size: 12px;
  line-height: 15px;
  color: #b4b4b4;
  border: none;
  height: 36px;
  width: 100%;

  &::after {
    content: "";
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center;
    background-image: url("~/assets/images/arrow.png");
    width: 15px;
    height: 10px;
  }

  @media (max-width: 768px) {
    width: auto;
  }
}

.select-options-list {
  position: absolute;
  z-index: 1;
  top: 35px;
  background: #fffefb;
  @include box-shadow(0px 2px 5px rgba(0, 0, 0, 0.1));
  border-radius: 4px;
  padding: 10px 16px;
  width: 100%;

  & * {
    cursor: pointer;
    font-size: 12px;

    &:not(:last-child) {
      margin-bottom: 10px;
    }
  }
}
</style>
