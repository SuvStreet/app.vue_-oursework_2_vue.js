<template>
  <div class="card container marginTitle">
    <h2 class="title">Сохрaнённые резюме</h2>
  </div>

  <div class="card container" v-for="{ id, ...value } in resumeProps" :key="id">
    <ul class="list">
      <li class="list-item">
        <div class="avatar clickAvatar" @click="$emit('visibleResume', id)">
          <div class="circle">
            <img :src="dataResume(value, 'avatar')" alt="avatar" />
          </div>
        </div>
        <div class="informationPerson">
          <h2>{{ dataResume(value, 'title') }}</h2>
          <small>{{ dataResume(value, 'text') }}</small>
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

export default {
  emits: ['visibleResume', 'removeResume'],
  props: ['resumeProps'],
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
        } else if (i > Object.keys(value).length) {
          switch (type) {
            case 'avatar':
              return 'https://i2.wp.com/www.cssscript.com/wp-content/uploads/2020/12/Customizable-SVG-Avatar-Generator-In-JavaScript-Avataaars.js.png?fit=438%2C408&ssl=1'
            case 'title':
              return 'Ваше имя'
            case 'text':
              return 'Описание вашего резюме'
          }
        }
      }
    },
    urlAvatar(value) {
      for (let i = 0; i < Object.keys(value).length; i++) {
        if (value[i].type === 'avatar') {
          return value[i].message
        } else if (i > Object.keys(value).length)
          return 'https://i2.wp.com/www.cssscript.com/wp-content/uploads/2020/12/Customizable-SVG-Avatar-Generator-In-JavaScript-Avataaars.js.png?fit=438%2C408&ssl=1'
      }
    },
    title(value) {
      for (let i = 0; i < Object.keys(value).length; i++) {
        if (value[i].type === 'title') {
          return value[i].message
        } else return 'Ваше имя'
      }
    },
    text(value) {
      for (let i = 0; i < Object.keys(value).length; i++) {
        if (value[i].type === 'text') {
          return value[i].message
        } else return 'Описание вашего резюме'
      }
    },
  },
  components: { AppButton },
}
</script>

<style scoped>
.clickAvatar {
  cursor: pointer;
}

.circle:hover {
  box-shadow: 0 0 10px 5px #2c3e50;
}

.marginTitle {
  margin-top: 15px;
}

.informationPerson {
  flex-basis: 65%;
}
</style>
