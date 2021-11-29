<template>
  <div class="card container margin-title">
    <h2 class="title">Сохрaнённые резюме</h2>
  </div>

  <div class="card container" v-for="{ id, ...value } in resumeProps" :key="id">
    <ul class="list">
      <li class="list-item">
        <app-avatar
          :message="dataResume(value, 'avatar')"
          @click="$emit('visibleResume', id)"
          class="clickEffect"
        ></app-avatar>
        <div class="information-person">
          <app-subtitle :message="dataResume(value, 'title')"></app-subtitle>
          <app-text :message="dataResume(value, 'text')"></app-text>
        </div>
        <app-button color="danger" @click="$emit('removeResume', id)">
          Удалить
        </app-button>
      </li>
    </ul>
  </div>
</template>

<script>
import AppButton from './AppButton.vue'
import AppAvatar from './AppAvatar.vue'
import AppText from './AppText.vue'
import AppSubtitle from './AppSubtitle.vue'

export default {
  emits: {
    visibleResume(id) {
      if (id) {
        return true
      }
      console.warn(
        'Не передан "id" "visibleResume" в компоненте "AppListResume"'
      )
      return false
    },
    removeResume(id) {
      if (id) {
        return true
      }
      console.warn(
        'Не передан "id" "removeResume" в компоненте "AppListResume"'
      )
      return false
    },
  },
  props: {
    resumeProps: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      regex: /^https?:\/\/.*(jpe?g|gif|png)/,
    }
  },
  methods: {
    dataResume(value, type) {
      for (let i = 0; i < Object.keys(value).length; i++) {
        if (value[i].type === type) {
          return value[i].message
        }
      }
    },
  },
  components: { AppButton, AppAvatar, AppText, AppSubtitle },
}
</script>

<style scoped>
.clickEffect:hover {
  cursor: pointer;
  box-shadow: 0 0 10px 5px #2c3e50;
  border-radius: 50%;
}

.information-person {
  flex-basis: 70%;
}

.btn {
  margin-left: 10px;
}
</style>
