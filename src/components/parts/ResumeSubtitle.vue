<template>
  <div class="card-w100">
    <h2>
      {{ message }}
    </h2>
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: false,
      default: 'Ваше имя',
    },
  },
}
</script>

<style scoped>
@media (max-width: 550px) {
  h2 {
    width: 100%;
    text-align: center;
  }
}
</style>
