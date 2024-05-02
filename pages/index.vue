<template>
  <login-user @evento="evento" />
</template>

<script>
import LoginUser from '~/components/users/LoginUser.vue'

export default {
  name: 'IndexPage',
  components: {
    LoginUser
  },
  layout: 'login',
  methods: {
    evento (data) {
      this.$nuxt.$emit('evento', data)
    }
  }
}
</script>
