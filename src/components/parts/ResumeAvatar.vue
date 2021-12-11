<template>
  <div class="card-w100">
    <div class="avatar">
      <div class="circle">
        <img :src="message" alt="avatar" />
      </div>
    </div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      required: false,
      default:
        'https://i2.wp.com/www.cssscript.com/wp-content/uploads/2020/12/Customizable-SVG-Avatar-Generator-In-JavaScript-Avataaars.js.png?fit=438%2C408&ssl=1',
    },
  },
}
</script>

<style scoped>
.avatar {
  flex-basis: 100%;
}
</style>
