<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="submited">
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
      <h3 v-if="dataBlock.length === 0">
        Добавьте первый блок, чтобы увидеть результат
      </h3>

      <div v-else v-for="(block, idx) in dataBlock" :key="idx">
        <component :is="componentName(idx)" :message="block.value"></component>
      </div>

      <app-button
        class="saveResume"
        v-if="dataBlock.length !== 0"
      >
        Сохранить резюме
      </app-button>
    </div>
  </div>

  <app-loader v-if="isLoader"></app-loader>

  <app-comments
    v-else-if="comments.length > 0"
    :commentsProps="comments"
  ></app-comments>

  <div class="container">
    <app-button
      v-if="comments.length === 0"
      color="primary"
      @action="loadComments"
    >
      Загрузить комментарии
    </app-button>

    <app-button
      color="primary"
    >
      Показать резюме
    </app-button>
  </div>
</template>

<script>
import AppTitle from './AppTitle.vue'
import AppSubtitle from './AppSubtitle.vue'
import AppAvatar from './AppAvatar.vue'
import AppText from './AppText.vue'
import AppComments from './AppComments.vue'
import AppLoader from './AppLoader.vue'
import AppButton from './AppButton.vue'
import axios from 'axios'

export default {
  data() {
    return {
      type: 'title',
      message: '',
      dataBlock: [],
      isLoader: false,
      comments: [],
    }
  },
  methods: {
    submited() {
      this.dataBlock.push({
        select: this.type,
        value: this.message,
      })
      this.type = 'title'
      this.message = ''
    },
    componentName(idx) {
      return 'app-' + this.dataBlock[idx].select
    },
    async loadComments() {
      // запускаем картинку загрузки
      this.isLoader = true

      const { data } = await axios.get(
        'https://jsonplaceholder.typicode.com/comments?_limit=41'
      )

      console.log(`data`, data)

      // если данные есть, преобразовываем в нужный формат
      this.comments = Object.keys(data).map((key) => {
        return {
          id: key,
          ...data[key],
        }
      })

      console.log(`this.comments`, this.comments)

      this.isLoader = false
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
    AppComments,
    AppLoader,
    AppButton,
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
