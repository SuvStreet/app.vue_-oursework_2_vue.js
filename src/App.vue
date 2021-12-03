<template>
  <div class="container column">
    <form v-if="isCreate" class="card card-w30" @submit.prevent="checkType">
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

      <app-button color="primary" :disabled-props="disabled">
        Добавить
      </app-button>
    </form>

    <div v-else class="card card-w30">
      <app-button class="container column" color="primary" @action="create">
        Создать резюме
      </app-button>
    </div>

    <div class="card card-w70 resume">
      <h3 v-if="resume.length === 0">
        Добавьте первый блок, чтобы увидеть результат
      </h3>

      <div v-else>
        <component
          v-for="{ id, type, message } in resume"
          :key="id"
          :is="componentName(type)"
          :message="message"
        >
          <app-button
            v-if="isSave"
            color="danger"
            @action="removeBlockResume(id)"
          >
            Удалить
          </app-button>
        </component>
      </div>

      <app-button class="saveResume" v-if="isSave" @action="saveResume">
        Сохранить резюме
      </app-button>
    </div>
  </div>

  <app-load
    @loadListComments="loadListComments"
    @loadListResume="loadListResume"
    :isOpenComments="isOpenComments"
    :isOpenResume="isOpenResume"
  ></app-load>

  <app-loader v-if="isLoader"></app-loader>

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

  <app-toast :toast="toast" @close="toast = {}"></app-toast>
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
import AppLoad from './AppLoad.vue'
import AppToast from './AppToast.vue'

import axios from 'axios'

export default {
  data() {
    return {
      type: 'title',
      message: '',
      resume: [],
      listComments: [],
      listResume: [],
      isOpenResume: false,
      isOpenComments: false,
      isSave: false,
      isLoader: false,
      isCreate: true,
      toast: {},
      regex: /^https?:\/\/.*(jpe?g|gif|png)/,
    }
  },
  mounted() {
    this.loadResume()
  },
  methods: {
    // =================================== ПРОВЕРКА КОРРЕКТНОГО URL КАРТИНКИ ===================================
    checkType() {
      if (this.type === 'avatar') {
        this.regex.test(this.message)
          ? this.submit()
          : (this.toast = {
              title: 'Информация',
              text: 'Укажите правильный URL аватара',
              type: 'info',
            })
      } else {
        this.submit()
      }
    },
    // =================================== ВЕРНУТЬ ТЕГ FORM ===================================
    create() {
      this.resume = []
      this.loadResume()
      this.isCreate = true
    },
    // =================================== СОЗДАЁМ БЛОК РЕЗЮМЕ ===================================
    async submit() {
      try {
        const response = await fetch(
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

        const firebaseData = await response.json()

        this.resume.push({
          id: firebaseData.name,
          type: this.type,
          message: this.message,
        })

        this.isSave = true

        this.type = 'title'
        this.message = ''
      } catch (error) {
        this.toast = {
          title: 'Ошибка',
          text: 'Создать блок резюме неудалось!',
          type: 'error',
        }
      }
    },
    // =================================== ЗАГРУЖАЕМ НЕ СОХРАНЁННОЕ РЕЗЮМЕ ===================================
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

        this.isSave = true
      } catch (error) {
        this.toast = {
          title: 'Ошибка',
          text: 'Нет, созданного блока резюме!',
          type: 'error',
        }
      }
    },
    // =================================== СОЗДАНИЕ ДИНАМИЧЕСКОГО КОМПОНЕНТА ===================================
    componentName(value) {
      return 'app-' + value
    },
    // =================================== ЗАГРУЖАЕМ СПИСОК КОММЕНТАРИЕВ ===================================
    async loadListComments() {
      try {
        if (!this.isOpenComments) {
          this.listResume = []
          this.isOpenResume = false
          this.isLoader = true

          const { data } = await axios.get(
            'https://jsonplaceholder.typicode.com/comments?_limit=41'
          )

          this.listComments = Object.keys(data).map((key) => {
            return {
              id: key,
              ...data[key],
            }
          })

          this.isOpenComments = true
        } else {
          this.listComments = []
          this.isOpenComments = false
        }
        this.isLoader = false
      } catch (error) {
        this.toast = {
          title: 'Ошибка',
          text: 'Нет комментариев!',
          type: 'error',
        }
      }
      this.isLoader = false
    },
    // =================================== ЗАГРУЖАЕМ СПИСОК РЕЗЮМЕ ===================================
    async loadListResume() {
      try {
        if (!this.isOpenResume) {
          this.listComments = []
          this.isOpenComments = false

          this.isLoader = true

          const { data } = await axios.get(
            'https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/save-resume.json'
          )

          this.listResume = Object.keys(data).map((key) => {
            return {
              id: key,
              ...data[key],
            }
          })
          this.isOpenResume = true
        } else {
          this.isOpenResume = false
          this.listResume = []
        }
      } catch (error) {
        this.toast = {
          title: 'Ошибка',
          text: 'Сохраннёных резюме нет!',
          type: 'error',
        }
      }
      this.isLoader = false
    },
    // =================================== УДАЛЯЕМ ОПРЕДЕЛЁННЫЙ БЛОК РЕЗЮМЕ ===================================
    async removeBlockResume(id) {
      try {
        this.resume = this.resume.filter((block) => block.id !== id)

        if (this.isSave) {
          await axios.delete(
            `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/resume/${id}.json`
          )
          if (this.resume.length === 0) {
            this.isSave = false
          }
        }
      } catch (error) {
        this.toast = {
          title: 'Ошибка',
          text: 'Удалить блок резюме неудалось!',
          type: 'error',
        }
      }
    },
    // =================================== УДАЛЯЕМ РЕЗЮМЕ ===================================
    async removeResume(id) {
      try {
        const resumeId = this.listResume.find((resume) => resume.id === id)
        await axios.delete(
          `https://vue-resume-database-default-rtdb.europe-west1.firebasedatabase.app/save-resume/${id}.json`
        )
        this.listResume = this.listResume.filter((resume) => resume.id !== id)

        this.toast = {
          title: 'Успешно',
          text: `Резюме "${resumeId[0].message}" удалено!`,
          type: 'success',
        }

        if (this.resume.length > 0) {
          this.resume = []
          this.isCreate = true
        }
      } catch (error) {
        this.toast = {
          title: 'Что-то пошло не так....',
          text: 'Резюме не удалено!',
          type: 'error',
        }
      }
    },
    // =================================== СОХРАНЯЕМ РЕЗЮМЕ ===================================
    async saveResume() {
      try {
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

        if (this.listResume.length !== 0) {
          this.listResume.push({
            id: firebaseData.name,
            ...this.resume,
          })
        }

        if (firebaseData) {
          this.toast = {
            title: 'Успешно',
            text: 'Резюме сохранено!',
            type: 'success',
          }
        }

        this.isSave = false
        this.resume = []
      } catch (error) {
        this.toast = {
          title: 'Что-то пошло не так....',
          text: 'Резюме не сохранено!',
          type: 'error',
        }
      }
    },
    // =================================== ПОКАЗЫВАЕМ ВЫБРАННОЕ РЕЗЮМЕ ===================================
    visibleResume(idx) {
      this.resume = Object.values(
        this.listResume.find((resume) => resume.id === idx)
      ).filter((key) => key !== idx)

      this.isCreate = false
      this.isSave = false
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
    AppLoad,
    AppToast,
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
</style>
