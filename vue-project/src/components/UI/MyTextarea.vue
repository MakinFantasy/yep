<template>
  <textarea
    name="textareaName"
    class="textarea"
    :value="modelValue"
    @input="updateTextarea"
  ></textarea>
</template>

<script>
export default {
  name: 'my-textarea',
  props: {
    textareaName: String,
    modelValue: String,
  },
  methods: {
    updateTextarea(event) {
      this.$emit('update:modelValue', event.target.value);
    },
  },
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/function' as *;
@use '@/assets/scss/variables' as *;
@use '@/assets/scss/mixin' as *;
.textarea {
  outline: color(primary);
  color: color(primary);
  font-size: 1.5rem;
  resize: none;
  width: 100%;
  border-radius: 5px;
  border: 2px solid color(primary);
  padding: 10px 15px;
  background-color: color(background-color);
  &:focus {
    box-shadow: 0px 0px 7px 3px #757575;
  }
}
</style>
