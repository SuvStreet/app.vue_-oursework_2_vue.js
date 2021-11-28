<template>
  <div class="container avatarCard">
    <div class="avatar">
      <div class="circle">
        <img :src="message" />
      </div>
    </div>
    <app-button color="danger" @click="$emit('removeBlockResume', )">
      Удалить
    </app-button>
  </div>
</template>

<script>
import AppButton from './AppButton.vue'

export default {
  emits: ['removeBlockResume'],
  props: ['message'],
  components: {
    AppButton,
  },
}
</script>

<style scoped>
.avatarCard {
  display: flex;
  align-items: center;
}

.avatar {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-basis: 90%;
}
</style>

//
https://cdn.dribbble.com/users/5592443/screenshots/14279501/drbl_pop_r_m_rick_4x.png
//
https://sun9-80.userapi.com/impg/0nQA12FuBusMcRE5uG8Fs_JOQF_-WjuMVBjuOg/1nrBNK-Zlyw.jpg?size=720x1080&quality=96&sign=9d72d4f0f1bd5c07d92fbb84a2204040&type=album
