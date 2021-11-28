<template>
  <div class="subtitle">
    <h2>
      {{ message }}
    </h2>
    <app-button color="danger" @click="$emits('removeBlockResume')">
      Удалить
    </app-button>
  </div>
</template>

<script>
import AppButton from './AppButton.vue'
export default {
  emits: ['removeBlockResume'],
  props: ['message'],
  components: {
    AppButton,
  },
}
</script>

<style scoped>
h2 {
  flex-basis: 75%;
}

.subtitle {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
