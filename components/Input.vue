<template>
  <div class="input-container">
    <span :class="{ 'input-name': true, required: isRequired }">
      {{ name }}
    </span>
    <textarea
      v-if="isMultiline"
      class="input multiline"
      :type="type"
      :placeholder="placeholder"
      @input="onChange"
    />
    <input
      v-else
      :class="{ input: true, error: meta.isError }"
      :type="type"
      :placeholder="placeholder"
      @input="onChange"
    >
    <p :class="{ 'input-error': true, show: meta.isError }">
      Поле является обязательным
    </p>
  </div>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      default: ''
    },
    field: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'number'
    },
    meta: {
      type: Object,
      default () {
        return {
          value: '',
          isError: false
        }
      }
    },
    placeholder: {
      type: String,
      default: ''
    },
    isRequired: {
      type: Boolean,
      default: false
    },
    isMultiline: {
      type: Boolean,
      default: false
    },
    callback: {
      type: Function,
      default: () => {}
    }
  },
  methods: {
    onChange (e) {
      const { value } = e.target
      this.callback(this.field, value)
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/css/mixins.scss";

.input-container {
  margin-bottom: 2px;
  & * {
    &:not(:last-child) {
      margin-bottom: 4px;
    }
  }
}

.input {
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border: 1px solid transparent;
  font-size: 12px;
  color: #535353;
  width:100%;
  max-width: 284px;
  padding: 9px 15px 10px;
  line-height: 15px;

  &::placeholder {
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 15px;
    color: #B4B4B4;
  }

  &.multiline {
    min-height: 108px;
    resize: none;
  }

  &.error {
    border: 1px solid #ff8484;
  }
}

.input::placeholder{
    margin-top: 10px;
    margin-left: 20px;
    color: #B4B4B4
}

.input-name {
  font-size: 10px;
  color: #49485e;
  display: flex;
  margin-bottom: 4px;
  letter-spacing: -0.02em;

  &.required {
    &:after {
      content: "";
      display: block;
      min-width: 4px;
      min-height: 4px;
      width: 4px;
      height: 4px;
      background: #ff8484;
      border-radius: 50%;
    }
  }
}

.input-error {
  font-size: 8px;
  color: #ff8484;
  opacity: 0;

  &.show {
    opacity: 1;
  }
}
</style>
