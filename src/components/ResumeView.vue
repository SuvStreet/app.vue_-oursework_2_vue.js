<template>
  <div class="card card-w70">
    <template v-if="blocks.length">
      <component
        v-for="{ id, type, message } in blocks"
        :key="id"
        :is="'resume-' + type"
        :message="message"
      >
        <base-button
          v-if="isSave"
          color="danger"
          @action="$emit('removeBlock', id)"
        >
          Удалить
        </base-button>
      </component>
      <div class="btnSaveResume" v-if="isSave">
        <base-button
          class="btnSaveResume"
          @action="$emit('saveResume')"
        >
          Сохранить резюме
        </base-button>
      </div>
    </template>

    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
import ResumeTitle from './parts/ResumeTitle.vue'
import ResumeSubtitle from './parts/ResumeSubtitle.vue'
import ResumeText from './parts/ResumeText.vue'
import ResumeAvatar from './parts/ResumeAvatar.vue'
import BaseButton from './BaseButton.vue'

export default {
  emits: ['removeBlock', 'saveResume'],
  props: ['blocks', 'isSave'],
  components: {
    ResumeTitle,
    ResumeSubtitle,
    ResumeText,
    ResumeAvatar,
    BaseButton,
  },
}
</script>

<style scoped>
.btnSaveResume {
  display: flex;
  justify-content: flex-end;
}

@media (max-width: 550px) {
  .btnSaveResume {
    justify-content: center;
    flex-basis: 100%;
  }
}
</style>
