<template>
  <v-app>
    <v-main style="background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR_b-v2owwcrqs0pdpd5Qsu1hiLRreHRUplIBlSRBIKbQ&s'); background-size: cover;">
      <ui-alert
        v-if="showAlert"
        :message="mensaje"
        :color="color"
        :type="type"
      />
      <v-row
        style="width: 100%; height: 100vh;"
        justify="center"
        align="center"
      >
        <Nuxt />
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      showAlert: false,
      mensaje: '',
      color: '',
      type: ''
    }
  },
  created () {
    this.$nuxt.$on('evento', (data) => {
      console.log('@@@ data login => ', data)
      this.mensaje = data.message
      this.color = data.color
      this.type = data.type
      this.showAlert = true
      setTimeout(() => {
        this.showAlert = false
      }, 5000)
    })
  },
  methods: {

  }
}
</script>
