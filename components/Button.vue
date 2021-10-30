<template>
  <button
    :class="{ 'submit-btn': true, disabled: !isFormValid }"
    @click="isFormValid ? callback() : () => {}"
  >
    {{ text }}
  </button>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      default: ''
    },
    isFormValid: {
      type: Boolean,
      default: false
    },
    callback: {
      type: Function,
      default: () => {}
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/css/mixins.scss";
.submit-btn {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 0px;
  background: #7bae73;
  @include box-shadow(0px 2px 4px rgba(0, 0, 0, 0.1));
  border-radius: 10px;
  color: #fff;
  border: none;
  cursor: pointer;

  &.disabled {
    background: #eeeeee;
    color: #b4b4b4;
    font-weight: 600;
    font-size: 12px;
    cursor: not-allowed;
  }
}
</style>
