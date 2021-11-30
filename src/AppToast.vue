<template>
  <div class="wrapper" v-if="toast">
    <div :class="['toast', toast.type, { 'show-toast': isOpenToast }]">
      <div class="container-1">
        <i :class="toastType"></i>
      </div>
      <div class="container-2">
        <p>{{ toast.title }}</p>
        <p>{{ toast.text }}</p>
      </div>
      <button @click="closeToast">&times;</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['toast'],
  data() {
    return {
      time: 0,
      isOpenToast: false,
    }
  },
  mounted() {
    setTimeout(() => {
      this.showToast()
    }, 200)
  },
  methods: {
    showToast() {
      this.isOpenToast = true
      this.time = setTimeout(() => {
        this.closeToast()
      }, 7000)
    },
    closeToast() {
      console.log(`closeToast`)
      this.isOpenToast = false
      clearTimeout(this.time)
    },
  },
  computed: {
    toastType() {
      return this.toast.type === 'success'
        ? 'fas fa-check-circle'
        : 'fas fa-times-circle'
    },
  },
}
</script>

<style scoped>
.wrapper {
  width: 420px;
  padding: 30px 20px;
  position: absolute;
  bottom: 50px;
  right: 0;
  overflow: hidden;
}
.wrapper .show-toast {
  transform: translateX(0);
}
.toast {
  width: 100%;
  height: 100px;
  padding: 20px;
  border-color: #fff;
  border-radius: 7px;
  display: grid;
  grid-template-columns: 1.3fr 6fr 0.5fr;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.08);
  background-color: #fff;
  transform: translateX(400px);
  transition: 1s;
}
.success {
  border-left: 8px solid #47f764;
}
.success i {
  color: #47f764;
}
.container-1 i {
  font-size: 35px;
}
.error {
  border-left: 8px solid #ff355b;
}
.error i {
  color: #ff355b;
}
.container-1,
container-2 {
  align-self: center;
}
.container-2 p:first-child {
  font-size: 16px;
  color: #101020;
  font-weight: 600;
  margin: 0;
}
.container-2 p:last-child {
  font-size: 12px;
  font-weight: 400;
  color: #656565;
  margin: 0;
}
.toast button {
  align-self: flex-start;
  background-color: transparent;
  border: none;
  font-size: 25px;
  line-height: 0;
  cursor: pointer;
  color: #656565;
}
</style>
