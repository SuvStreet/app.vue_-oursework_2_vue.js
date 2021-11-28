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
  props: ['message'],
}
</script>

<style scoped>
h2 {
  flex-basis: 100%;
}

.subtitle {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
