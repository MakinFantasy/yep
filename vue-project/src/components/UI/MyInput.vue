<template>
  <input
    :type="inputType"
    :name="inputName"
    :value="modelValue"
    @input="updateInput"
    class="input"
  />
</template>

<script>
export default {
  name: 'my-input',

  props: {
    inputType: String,
    inputName: String,
    modelValue: String,
  },

  methods: {
    updateInput(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/function' as *;
@use '@/assets/scss/variables' as *;
@use '@/assets/scss/mixin' as *;

.input {
  color: color(primary);
  font-size: 1.5rem;
  width: 100%;
  padding: 10px 15px;
  border-radius: 5px;
  border: 2px solid color(primary);
  background: color(background-color);

  &:focus {
    outline: color(primary);
    box-shadow: 0px 0px 4px 1px color(primary);
  }
}
</style>
