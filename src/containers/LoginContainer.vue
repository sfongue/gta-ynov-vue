<template>
  <div class="app">
    <div class="container-fluid">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>

export default {
  name: 'LoginContainer',
  components: {
    
  },
  data () {
    return {
      nav: nav.items
    }
  },
  computed: {
    name () {
      return this.$route.name
    },
    list () {
      return this.$route.matched.filter((route) => route.name || route.meta.label )
    }
  }
}
</script>
