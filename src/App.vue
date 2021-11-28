<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="submit">
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

      <app-button color="primary" :disabledProps="disabled">
        Добавить
      </app-button>
    </form>

    <div class="card card-w70 resume">
      <h3 v-if="resume.length === 0">
        Добавьте первый блок, чтобы увидеть результат
      </h3>

      <div v-else>
        <component
          v-for="{ id, type, message } in resume" :key="id"
          :is="componentName(type)"
          :message="message"
          @removeBlockResume="removeBlockResume(id)"
        ></component>
      </div>

      <app-button
        class="saveResume"
        v-if="resume.length !== 0"
        @action="saveResume"
      >
        Сохранить резюме
      </app-button>
    </div>
  </div>

  <app-loader v-if="isLoader"></app-loader>

  <div class="container divLoad">
    <app-button
      v-if="listComments.length === 0"
      color="primary"
      @action="loadComments"
    >
      Загрузить комментарии
    </app-button>

    <app-button
      color="primary"
      v-if="listResume.length === 0"
      @action="loadListResume"
    >
      Показать резюме
    </app-button>
  </div>

  <app-list-comments
    v-if="listComments.length > 0"
    :commentsProps="listComments"
  ></app-list-comments>

  <app-list-resume
    v-if="listResume.length > 0"
    :resumeProps="listResume"
    @visibleResume="visibleResume"
    @removeResume="removeResume"
  ></app-list-resume>
</template>

<script>
import AppTitle from './AppTitle.vue'
import AppSubtitle from './AppSubtitle.vue'
import AppAvatar from './AppAvatar.vue'
import AppText from './AppText.vue'
import AppListComments from './AppListComments.vue'
import AppLoader from './AppLoader.vue'
import AppButton from './AppButton.vue'
import AppListResume from './AppListResume.vue'

import axios from 'axios'

export default {
  data() {
    return {
      type: 'title',
      message: '',
      resume: [],
      blockResume: [],
      isLoader: false,
      listComments: [],
      listResume: [],
      idResume: null,
    }
  },
  mounted() {
    this.loadResume()
  },
  methods: {
    async submit() {
      await fetch(
        `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/resume.json`,
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            type: this.type,
            message: this.message,
          }),
        }
      )

      this.loadResume()

      this.type = 'title'
      this.message = ''
    },
    async loadResume() {
      try {
        const { data } = await axios.get(
          'https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/resume.json'
        )
        this.resume = Object.keys(data).map((key) => {
          return {
            id: key,
            ...data[key],
          }
        })
      } catch (error) {
        console.log('Давайте составим резюме!')
      }
    },
    componentName(value) {
      return 'app-' + value
    },
    async loadComments() {
      if (this.listResume.length !== 0) {
        this.listResume = []
      }

      // запускаем картинку загрузки
      this.isLoader = true

      const { data } = await axios.get(
        'https://jsonplaceholder.typicode.com/comments?_limit=41'
      )

      //console.log(`data`, data)

      // если данные есть, преобразовываем в нужный формат
      this.listComments = Object.keys(data).map((key) => {
        return {
          id: key,
          ...data[key],
        }
      })

      //console.log(`this.comments`, this.comments)

      this.isLoader = false
    },
    // =================================== УДАЛЯЕМ ОПРЕДЕЛЁННЫЙ БЛОК РЕЗЮМЕ ===================================
    async removeBlockResume(id) {

      console.log(`id`, id)
      // await axios.delete(
      //   `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/resume/${id}.json`
      // )
      // this.resume = this.resume.filter((block) => block.id !== id)
    },
    // =================================== УДАЛЯЕМ РЕЗЮМЕ ===================================
    async removeResume(id) {
      await axios.delete(
        `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/save-resume/${id}.json`
      )
      this.listResume = this.listResume.filter((resume) => resume.id !== id)

      if (this.resume.length > 0) {
        this.resume = []
      }
    },
    // =================================== ЗАГРУЖАЕМ СПИСОК РЕЗЮМЕ ===================================
    async loadListResume() {
      if (this.listComments.length !== 0) {
        this.listComments = []
      }

      this.isLoader = true

      try {
        const { data } = await axios.get(
          'https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/save-resume.json'
        )

        this.listResume = Object.keys(data).map((key) => {
          return {
            id: key,
            ...data[key],
          }
        })
      } catch (error) {
        console.log('Сохраннённых резюме нет!')
      }

      // this.listResume = data

      // console.log(`data`, data)
      // console.log(`newBlockResume`, newBlockResume)
      // console.log(`list-resume`, this.listResume)

      this.isLoader = false
    },
    // =================================== СОХРАНЯЕМ РЕЗЮМЕ ===================================
    async saveResume() {
      await axios.delete(
        'https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/resume.json'
      )
      const response = await fetch(
        `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/save-resume.json`,
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.resume),
        }
      )

      const firebaseData = await response.json()

      if (this.listResume.length > 0) {
        this.listResume.push({
          id: firebaseData.name,
          ...Object.keys(this.resume).map((key) => {
            return {
              id: key,
              ...this.resume[key],
            }
          }),
        })
      }

      this.resume = []
    },
    // =================================== ПОКАЗЫВАЕМ ВЫБРАННОЕ РЕЗЮМЕ ===================================
    async visibleResume(idx) {
      const { data } = await axios.get(
        `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/save-resume/${idx}.json`
      )
      this.resume = Object.keys(data).map((key) => {
        return {
          id: key,
          ...data[key],
        }
      })

      // console.log(`idx`, idx)

      // console.log(`data`, `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/save-resume/-MpT4g8eSuVOwyZRn7Ed.json`)
    },
  },
  computed: {
    disabled() {
      return this.message.length < 4 ? true : false
    },
  },
  components: {
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText,
    AppListComments,
    AppLoader,
    AppButton,
    AppListResume,
  },
}
</script>

<style>
.saveResume {
  align-self: flex-end;
  margin-top: 0;
}

.resume {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.divLoad {
  margin-bottom: 10px;
}
</style>
