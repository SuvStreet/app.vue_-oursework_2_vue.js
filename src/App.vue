<template>
  <div class="container column">
    <resume-form
      :isCreateProps="isCreate"
      @block-added="addBlock"
      @toast="openToast"
      @submit="submit"
      @onCreate="create"
    ></resume-form>

    <resume-view
      :blocks="blocks"
      :isSave="isSave"
      @saveResume="saveResume"
      @removeBlock="removeBlock"
    ></resume-view>
  </div>

  <list-group-button
    @loadListComments="loadListComments"
    @loadListResume="loadListResume"
    :isOpenComments="isOpenComments"
    :isOpenResume="isOpenResume"
  ></list-group-button>

  <app-loader v-if="isLoader"></app-loader>

  <list-comments
    v-if="listComments.length"
    :commentsProps="listComments"
  ></list-comments>

  <list-resume
    v-if="listResume.length"
    :resumeProps="listResume"
    @resume="visibleResume"
    @removeResume="removeResume"
  ></list-resume>

  <app-toast :toast="toast" @close="toast = {}"></app-toast>
</template>

<script>
import AppLoader from './AppLoader.vue'
import AppToast from './AppToast.vue'
import ListGroupButton from './components/list/ListGroupButton.vue'
import ListComments from './components/list/ListComments.vue'
import ListResume from './components/list/ListResume.vue'
import ResumeForm from './components/ResumeForm.vue'
import ResumeView from './components/ResumeView.vue'

import axios from 'axios'

export default {
  data() {
    return {
      blocks: [],
      listComments: [],
      listResume: [],
      isOpenResume: false,
      isOpenComments: false,
      isSave: false,
      isLoader: false,
      isCreate: true,
      toast: {},
    }
  },
  mounted() {
    this.loadResume()
  },
  methods: {
    addBlock(block) {
      this.blocks.push(block)
    },
    openToast(value) {
      this.toast = value
    },
    // =================================== ВЕРНУТЬ ТЕГ FORM ===================================
    create() {
      this.blocks = []
      this.loadResume()
      this.isCreate = true
    },
    // =================================== СОЗДАЁМ БЛОК РЕЗЮМЕ ===================================
    async submit(type, message) {
      try {
        const response = await fetch(process.env.VUE_APP_URL_RESUME + '.json', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            type,
            message,
          }),
        })

        const firebaseData = await response.json()

        this.blocks.push({
          id: firebaseData.name,
          type,
          message,
        })

        this.isSave = true
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
          process.env.VUE_APP_URL_RESUME + '.json'
        )

        this.blocks = Object.keys(data).map((key) => {
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
            process.env.VUE_APP_URL_SAVE_RESUME + '.json'
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
    async removeBlock(id) {
      try {
        if (this.isSave) {
          await axios.delete(process.env.VUE_APP_URL_RESUME + `/${id}.json`)
          if (this.blocks.length === 0) {
            this.isSave = false
          }
        }

        this.blocks = this.blocks.filter((block) => block.id !== id)
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
        const resumeId = this.listResume.find((blocks) => blocks.id === id)

        await axios.delete(process.env.VUE_APP_URL_SAVE_RESUME + `/${id}.json`)
        this.listResume = this.listResume.filter((blocks) => blocks.id !== id)

        this.toast = {
          title: 'Успешно',
          text: `Резюме "${
            resumeId[0].message.length > 50
              ? resumeId[0].type
              : resumeId[0].message
          }" удалено!`,
          type: 'success',
        }

        if (this.blocks.length) {
          this.blocks = []
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
        await axios.delete(process.env.VUE_APP_URL_RESUME + '.json')
        const response = await fetch(
          process.env.VUE_APP_URL_SAVE_RESUME + '.json',
          {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(this.blocks),
          }
        )

        const firebaseData = await response.json()

        if (this.listResume.length !== 0) {
          this.listResume.push({
            id: firebaseData.name,
            ...this.blocks,
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
        this.blocks = []
      } catch (error) {
        this.toast = {
          title: 'Что-то пошло не так....',
          text: 'Резюме не сохранено!',
          type: 'error',
        }
      }
    },
    // =================================== ПОКАЗЫВАЕМ ВЫБРАННОЕ РЕЗЮМЕ ===================================
    visibleResume(obj) {
      this.blocks = obj

      this.isCreate = false
      this.isSave = false
    },
  },
  components: {
    AppLoader,
    AppToast,
    ResumeForm,
    ResumeView,
    ListGroupButton,
    ListComments,
    ListResume,
  },
}
</script>
