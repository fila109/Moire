<template>
  <label class="form__label">
    <slot></slot>
    <span class="error" style="color: red;" v-if="error">{{ error }}</span>
  </label>
</template>

<script>
export default {
  props: ['title', 'error'],
};
</script>