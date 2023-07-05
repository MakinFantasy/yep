<template>
  <button class="button">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-button',
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/function' as *;
@use '@/assets/scss/variables' as *;
@use '@/assets/scss/mixin' as *;

.button {
  padding: 15px;
  color: color(button-text-color);
  background-color: color(button-color);
  border-radius: 10px;
  transition: all 0.3s ease;
  @include media(min, md) {
    padding: 15px;
  }

  &:focus {
    box-shadow: 0px 0px 7px 3px #757575;
  }

  &:active {
    background-color: color(button-active);
  }

  &[disabled] {
    transform: scale(0.9);
    opacity: 0.7;
  }

  &[disabled]:active {
    background-color: color(disabled-color);
  }
}
</style>
