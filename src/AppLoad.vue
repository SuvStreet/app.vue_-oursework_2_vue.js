<template>
  <div class="container card center">
    <div>
      <h4>
        {{ isOpenTitle() }}
      </h4>
      <div class="groupBtn">
        <app-button
          @action="$emit('loadListComments')"
          :color="isOpenComments ? 'danger' : ''"
        >
          {{ isOpenComments ? 'Закрыть комментарии' : 'Загрузить комментарии' }}
        </app-button>

        <app-button
          @action="$emit('loadListResume')"
          :color="isOpenResume ? 'danger' : ''"
        >
          {{ isOpenResume ? 'Закрыть резюме' : 'Загрузить резюме' }}
        </app-button>
      </div>
    </div>
  </div>
</template>

<script>
import AppButton from './AppButton.vue'

export default {
  emits: ['loadListComments', 'loadListResume'],
  props: {
    isOpenComments: {
      type: Boolean,
      default: false,
    },
    isOpenResume: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    isOpenTitle() {
      if (this.isOpenComments) {
        return 'Комментарии'
      } else if (this.isOpenResume) {
        return 'Резюме'
      } else {
        return 'Какую информацию вы хотите получить?'
      }
    },
  },
  components: {
    AppButton,
  },
}
</script>

<style scoped>
h4 {
  text-align: center;
}

.divLoad {
  margin-bottom: 10px;
}
</style>
