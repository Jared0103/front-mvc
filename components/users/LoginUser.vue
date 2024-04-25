<template>
  <v-card color="blue-grey lighten-4" elevation="0" width="500">
    <v-card-title>
      Iniciar Sesión
    </v-card-title>
    <v-card-text>
      <v-form ref="form" v-model="validForm">
        Email:
        <v-text-field v-model="email" placeholder="Escribe tu correo" type="email" :rules="correo" />
        Password:
        <v-text-field v-model="passwordUser" placeholder="Escribe tu password" type="password" :rules="password" />
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-row>
        <v-col cols="6">
          <v-btn block color="#81C784" elevation="0" @click="loginUser">
            <span style="text-transform: none; color: white;">
              Login
            </span>
          </v-btn>
        </v-col>
        <v-col cols="6">
          <v-btn block color="#FF9800" elevation="0" @click="signupUser">
            <span style="text-transform: none; color: white;">
              Signup
            </span>
          </v-btn>
        </v-col>
      </v-row>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      validForm: false,
      email: null,
      passwordUser: null,
      required: [
        v => !!v || 'Campo obligatorio'
      ],
      password: [
        v => (v && v.length > 5) || 'La contraseña debe tener más de 6 caracteres'
      ],

      correo: [
        v => /.+@.+\..+/.test(v) || 'Email must be valid'
      ]
    }
  },
  methods: {
    loginUser () {
      if (this.$refs.form) {
        this.validForm = this.$refs.form.validate()
        if (this.validForm) {
          const sendData = {
            email: this.email,
            password: this.passwordUser
          }
          const url = '/login'
          this.$axios.post(url, sendData)
            .then((res) => {
              console.log('@@ res => ', res)
              if (res.data.token) {
                localStorage.setItem('token', res.data.token)
                this.$router.push('/principal')
              }
            })
            .catch((err) => {
              console.error('@@ err => : ', err)
            })
        } else {
          alert('Algo está mal')
        }
      } else {
        console.error('Referencia de formulario no encontrada')
      }
    },
    signupUser () {
      // Lógica para registrarse
    }
  }
}
</script>
