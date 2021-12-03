<template>
  <div class="wrapper">
    <div :class="['toast', newToast.type, { 'show-toast': isOpenToast }]">
      <div class="container-1">
        <i :class="toastType"></i>
      </div>
      <div class="container-2">
        <p>{{ newToast.title }}</p>
        <p>{{ newToast.text }}</p>
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
      newToast: {},
    }
  },
  methods: {
    showToast() {
      this.newToast = this.toast
      this.isOpenToast = true
      this.time = setTimeout(() => {
        this.closeToast()
      }, 7000)
    },
    closeToast() {
      this.isOpenToast = false
      clearTimeout(this.time)
    },
  },
  computed: {
    toastType() {
      if (this.newToast.type === 'success') {
        return 'fas fa-check-circle'
      } else if (this.newToast.type === 'error') {
        return 'fas fa-times-circle'
      } else {
        return 'fas fa-info-circle'
      }
    },
  },
  watch: {
    toast() {
      if (this.isOpenToast) {
        this.closeToast()
        setTimeout(() => {
          this.showToast()
        }, 1000)
      } else this.showToast()
    },
  },
}
</script>

<style scoped>
.wrapper {
  position: fixed;
  width: 420px;
  padding: 30px 20px;
  bottom: 50px;
  right: 0;
  overflow: hidden;
  pointer-events: none;
}

@media (max-width: 450px) {
  .wrapper {
    width: 320px;
    padding: 20px 10px;
  }
  .container-2 {
    padding-left: 10px;
  }
}

.wrapper .show-toast {
  transform: translateX(0);
  pointer-events: all;
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
.info {
  border-left: 8px solid #6a45ff;
}
.info i {
  color: #6a45ff;
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
  pointer-events: all;
  align-self: flex-start;
  background-color: transparent;
  border: none;
  font-size: 25px;
  line-height: 0;
  cursor: pointer;
  color: #656565;
}
</style>
