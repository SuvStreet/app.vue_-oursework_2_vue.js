<template>
  <div class="avatar">
    <div class="circle">
      <img
        :src="message"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: ['message'],
  data() {},
}
</script>

<style scoped>
.avatar {
  display: flex;
  justify-content: center;
  align-items: center;
}

.circle {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  overflow: hidden;
  border: 1px solid #e6e6e6;
}

.avatar img {
  width: 150px;
  height: auto;
  margin-top: -15%;
}
</style>

//
https://cdn.dribbble.com/users/5592443/screenshots/14279501/drbl_pop_r_m_rick_4x.png
//
https://sun9-80.userapi.com/impg/0nQA12FuBusMcRE5uG8Fs_JOQF_-WjuMVBjuOg/1nrBNK-Zlyw.jpg?size=720x1080&quality=96&sign=9d72d4f0f1bd5c07d92fbb84a2204040&type=album
