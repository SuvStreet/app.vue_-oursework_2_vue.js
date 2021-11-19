<template>
  <div>
    <component :is="selectComponent" :messageProps="props"></component>
  </div>
</template>

<script>
import AppTitle from './AppTitle.vue'
import AppSubtitle from './AppSubtitle.vue'

export default {
  props: ['props'],
  data() {
    return {}
  },
  methods: {
    selectComponent() {
      for (let key = 0; key < this.props.length; key++) {
        console.log(`object`, this.props[key].select)
        if (this.props[key].select === 'AppTitle') {
          'AppTitle'
        } else if (this.props[key].select === 'AppSubtitle') {
          'AppSubtitle'
        }
      }
    },
  },
  computed: {
    // componentName(com){
    //   return "App" + com
    // }
  },
  components: {
    AppTitle,
    AppSubtitle,
  },
}
</script>
<style lang=""></style>
