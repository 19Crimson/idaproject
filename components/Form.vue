<template>
  <div class="sidebar">
    <input-text
      :meta="sidebarData.name"
      field="name"
      placeholder="Введите наименование товара"
      name="Наименование товара"
      type="text"
      :callback="onChangeInput"
      is-required
    />
    <input-text
      :meta="sidebarData.description"
      field="description"
      placeholder="Введите описание товара"
      name="Описание товара"
      type="text"
      :callback="onChangeInput"
      is-multiline
    />
    <input-text
      :meta="sidebarData.imageLink"
      field="imageLink"
      type="text"
      name="Ссылка на изображение товара"
      placeholder="Введите ссылку"
      :callback="onChangeInput"
      is-required
    />
    <input-text
      :val="sidebarData.price"
      field="price"
      type="number"
      name="Цена товара"
      placeholder="Введите цену"
      :callback="onChangeInput"
      is-required
    />
    <submit-button
      :callback="createProduct"
      :is-form-valid="isFormValid"
      text="Добавить товар"
    />
  </div>
</template>

<script>
import Input from './Input.vue'
import Button from './Button.vue'

export default {
  components: {
    'input-text': Input,
    'submit-button': Button
  },
  props: {
    onCreate: {
      type: Function,
      default () {
        return { }
      }
    }
  },
  data () {
    return {
      sidebarData: {
        name: {
          value: '',
          isError: false
        },
        description: {
          value: '',
          isError: false
        },
        imageLink: {
          value: '',
          isError: false
        },
        price: {
          value: '',
          isError: false
        }
      },
      isFormValid: false
    }
  },
  methods: {
    onChangeInput (type, value) {
      this.sidebarData[type] = {
        value,
        isError: type !== 'description' && value === ''
      }
      this.isFormValid = Object.keys(this.sidebarData).every(
        key => this.sidebarData[key].value !== '' || key === 'description'
      )
    },
    createProduct () {
      this.onCreate(this.sidebarData).then(() => {
        this.clearForm()
      })
    },
    clearForm () {
      Object.keys(this.sidebarData).forEach((key) => {
        this.sidebarData[key] = { value: '', isError: false }
      })
      this.isFormValid = false
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/css/mixins.scss";

.sidebar {
    width: 100%;
    height: 100%;
    background: #FFFEFB;
    @include box-shadow(0px 20px 30px rgba(0, 0, 0, 0.04),
        0px 6px 10px rgba(0, 0, 0, 0.02));
    border-radius: 4px;
    padding: 24px;
}

.input::placeholder {
  font-size: 12px;
}

.submit-btn {
  margin-top: 8px;
}
</style>
