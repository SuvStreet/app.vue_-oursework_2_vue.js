<template>
  <div class="card container" v-for="{ id, ...value } in resumeProps" :key="id">
    <ul class="list">
      <li class="list-item">
        <resume-avatar
          :message="dataResume(value, 'avatar')"
          @click="visibleResume(value)"
          class="clickEffect"
        ></resume-avatar>
        <div class="information-person">
          <resume-subtitle
            :message="dataResume(value, 'title')"
          ></resume-subtitle>
          <resume-text :message="dataResume(value, 'text')"></resume-text>
        </div>
        <base-button color="danger" @click="$emit('removeResume', id)">
          Удалить
        </base-button>
      </li>
    </ul>
  </div>
</template>

<script>
import BaseButton from '../BaseButton.vue'
import ResumeAvatar from '../parts/ResumeAvatar.vue'
import ResumeText from '../parts/ResumeText.vue'
import ResumeSubtitle from '../parts/ResumeSubtitle.vue'

export default {
  emits: {
    resume(obj) {
      if (obj) {
        return true
      }
      console.warn('Не передан "obj" "resume" в компоненте "AppListResume"')
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
  methods: {
    dataResume(value, type) {
      for (let i = 0; i < Object.keys(value).length; i++) {
        if (value[i].type === type) {
          return value[i].message
        }
      }
    },
    visibleResume(obj) {
      this.$emit(
        'resume',
        Object.keys(obj).map((key) => {
          return {
            ...obj[key],
          }
        }),
      )
    },
  },
  components: { BaseButton, ResumeAvatar, ResumeText, ResumeSubtitle },
}
</script>

<style scoped>
.clickEffect:hover {
  cursor: pointer;
  box-shadow: 0 0 10px 5px #2c3e50;
  border-radius: 50%;
}

.information-person {
  width: 100%;
  margin: 0 10px;
}

.list-item .btn {
  margin-right: 0;
}
</style>
