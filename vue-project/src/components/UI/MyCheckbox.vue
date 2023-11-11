<template>
  <label class="checkbox">
    <span class="visually-hidden">Переключатель</span>
    <input
      type="checkbox"
      :checked="checked"
      @input="updateCheck"
      class="visually-hidden checkbox__input"
    />
    <span class="checkbox__span"></span>
  </label>
</template>

<script>
export default {
  name: 'my-checkbox',
  
  props: {
    checked: Boolean,
  },

  methods: {
    updateCheck() {
      this.$emit('check');
    },
  },
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/function' as *;
@use '@/assets/scss/variables' as *;
@use '@/assets/scss/mixin' as *;

.checkbox__span {
  position: relative;
  display: inline-block;
  width: 34px;
  height: 34px;
  cursor: pointer;
  background-color: color(background-color);
  transition: 0.4s;
  border-radius: 10px;
  border: 1px dotted color(primary);
}

.checkbox__span:before {
  content: '';
  position: absolute;
  top: 50%;
  left: 17px;
  transform: translate(-50%, -50%) rotate(45deg);
  width: 12px;
  height: 24px;
  border: solid white;
  border-width: 0 4px 4px 0;
  opacity: 0;
}
input:checked + .checkbox__span {
  background-color: color(button-color);
}

input:focus + .checkbox__span {
  box-shadow: 0 0 1px 2px color(pure-white);
}

input:checked + .checkbox__span:before {
  opacity: 1;
}
</style>
