<template>
  <div class="subtitle">
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
.subtitle {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

@media (max-width: 700px) {
  .subtitle {
    flex-direction: column;
  }
}
</style>
