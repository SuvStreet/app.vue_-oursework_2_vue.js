<template>
  <button
    class="btn"
    :class="color"
    @click="$emit('action')"
    :disabled="disabledProps"
  >
    <slot>
      {{ buttonText }}
    </slot>
  </button>
</template>

<script>
export default {
  emits: ['action'],
  props: {
    buttonText: {
      type: String,
      default: 'Кнопка',
    },
    color: {
      type: String,
      default: '',
    },
    disabledProps: {
      type: Boolean,
      default: false,
    },
  },
}
</script>

<style></style>

// Загрузить комментарии
