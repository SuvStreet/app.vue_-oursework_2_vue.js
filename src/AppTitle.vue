<template>
  <div class="container column">
    <h1>{{ message }}</h1>
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: true,
    },
  },
}
</script>

<style scoped>
.container .column {
  justify-content: space-between;
  align-items: center;
}
</style>
