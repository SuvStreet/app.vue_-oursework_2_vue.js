<template>
  <form class="card card-w30" @submit.prevent="checkType">
    <div v-if="isCreateProps">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="type">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model.trim="message"></textarea>
      </div>

      <base-button color="primary" :disabled="isDisabled">
        Добавить
      </base-button>
    </div>

    <base-button
      v-else
      class="container column"
      color="primary"
      @action="$emit('onCreate')"
    >
      Создать резюме
    </base-button>
  </form>
</template>

<script>
import BaseButton from './BaseButton.vue'

export default {
  emits: ['toast', 'submit', 'onCreate'],
  props: ['isCreateProps'],
  data() {
    return {
      type: 'title',
      message: '',
    }
  },
  methods: {
    // =================================== ПРОВЕРКА КОРРЕКТНОГО URL КАРТИНКИ ===================================
    checkType() {
      const regex = /^https?:\/\/.*(jpe?g|gif|png)/

      if (this.type === 'avatar') {
        regex.test(this.message)
          ? this.emitSubmit()
          : this.$emit('toast', {
              title: 'Информация',
              text: 'Укажите правильный URL аватара',
              type: 'info',
            })
      } else {
        this.emitSubmit()
      }
    },
    emitSubmit() {
      this.$emit('submit', this.type, this.message)
      this.type = 'title'
      this.message = ''
    },
  },
  computed: {
    isDisabled() {
      return this.message.length < 4
    },
  },
  components: {
    BaseButton,
  },
}
</script>

<style></style>
