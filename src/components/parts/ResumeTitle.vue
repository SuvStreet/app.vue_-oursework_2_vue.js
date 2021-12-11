<template>
  <div class="card-w100">
    <h1 class="h1-center">{{ message }}</h1>
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      default: 'Ваш title!'
    }
  }
}
</script>

<style scoped>
.h1-center {
  flex-basis: 100%;
  text-align: center;
}

@media (max-width: 700px) {
  .container.column {
    justify-content: center;
  }
}
</style>
