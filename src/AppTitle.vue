<template>
  <div class="container title">
    <h1 class="text">{{ message }}</h1>
    <app-button color="danger" @click="$emit('removeBlockResume')">
      Удалить
    </app-button>
  </div>
</template>

<script>
import AppButton from './AppButton.vue'

export default {
  emits: ['removeBlockResume'],
  props: {
    message: {
      type: String,
      required: true,
    },
  },
  components: {
    AppButton,
  },
}
</script>

<style>
.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.text {
  text-align: center;
  flex-basis: 90%;
}
</style>
