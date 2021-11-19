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

      <button class="btn primary" :disabled="disabled">Добавить</button>
    </form>

    <div class="card card-w70">
      <h3 v-if="dataBlock.length === 0">
        Добавьте первый блок, чтобы увидеть результат
      </h3>

      <div v-else v-for="(block, idx) in dataBlock" :key="idx">
        <component :is="componentName(idx)" :message="block.value"></component>
      </div>
    </div>
  </div>

  <app-loader v-if="isLoader"></app-loader>
  <app-comments
    v-else-if="comments.length > 0"
    :commentsProps="comments"
  ></app-comments>

  <div class="container">
    <p>
      <button class="btn primary" @click="loadComments">
        Загрузить комментарии
      </button>
    </p>
  </div>
</template>

<script>
import AppTitle from './AppTitle.vue'
import AppSubtitle from './AppSubtitle.vue'
import AppAvatar from './AppAvatar.vue'
import AppText from './AppText.vue'
import AppComments from './AppComments.vue'
import AppLoader from './AppLoader.vue'
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
      return this.message.length < 4
    },
  },
  components: {
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText,
    AppComments,
    AppLoader,
  },
}
</script>

<style></style>
