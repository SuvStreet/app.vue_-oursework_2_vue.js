<template>
  <div class="text">
    <p>
      {{ message }}
    </p>
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: false,
      default: 'Описание вашего резюме',
    },
  },
}
</script>

<style scoped>
.text {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
