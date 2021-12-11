<template>
  <div class="card container" v-for="{ id, email,  body} in commentsProps" :key="id">
    <ul class="list">
      <li class="list-item">
        <resume-avatar></resume-avatar>
        <div class="information-person">
          <resume-subtitle
            :message="email"
          ></resume-subtitle>
          <resume-text :message="body"></resume-text>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import ResumeAvatar from '../parts/ResumeAvatar.vue'
import ResumeSubtitle from '../parts/ResumeSubtitle.vue'
import ResumeText from '../parts/ResumeText.vue'

export default {
  props: {
    commentsProps: {
      type: Array,
      required: true,
    },
  },
  components: {
    ResumeAvatar,
    ResumeSubtitle,
    ResumeText,
  },
}
</script>
