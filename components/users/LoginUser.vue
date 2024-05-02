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
        <v-text-field v-model="passwordUser" placeholder="Escribe tu contraseña" type="password" :rules="password" />
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
          <v-btn block color="#FF9800" elevation="0" @click="showNuevo = true">
            <span style="text-transform: none; color: white;">
              Signup
            </span>
          </v-btn>
        </v-col>
      </v-row>
    </v-card-actions>
    <!-- Diálogo de registro de usuario -->
    <v-dialog v-model="showNuevo" width="400" persistent>
      <v-card>
        <v-card-title>Registrar Usuario</v-card-title>
        <v-card-text>
          <v-form ref="formNuevo" v-model="validFormNuevo">
            Email:
            <v-text-field v-model="emailNuevo" placeholder="Escribe tu correo" type="email" :rules="correo" />
            Password:
            <v-text-field v-model="passwordUserNuevo" placeholder="Escribe tu contraseña" type="password" :rules="password" />
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-row>
            <v-col cols="6">
              <!-- Botón para agregar usuario -->
              <v-btn block color="primary" @click="agregar">
                <span class="white--text">Agregar</span>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <!-- Botón para cancelar el registro -->
              <v-btn block color="red" @click="showNuevo = false">
                <span class="white--text">Cancelar</span>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'

export default {
  data () {
    return {
      // Datos de inicio de sesión
      validForm: false,
      email: null,
      passwordUser: null,
      // Datos de registro de usuario
      validFormNuevo: false,
      emailNuevo: null,
      passwordUserNuevo: null,
      // Regula la visibilidad del diálogo de registro de usuario
      showNuevo: false,
      // Reglas de validación
      password: [
        v => (v && v.length > 5) || 'La contraseña debe tener al menos 6 caracteres'
      ],
      correo: [
        v => /.+@.+\..+/.test(v) || 'El correo electrónico debe ser válido'
      ]
    }
  },
  methods: {
    // Método para iniciar sesión
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
    // Método para registrar un nuevo usuario
    agregar () {
      this.validFormNuevo = this.$refs.formNuevo.validate()
      if (this.validFormNuevo) {
        const sendData = {
          id: uuidv4(),
          email: this.emailNuevo,
          password: this.passwordUserNuevo
        }
        console.log('@@@ data =>', sendData)
        const url = '/signup'
        this.$axios.post(url, sendData)
          .then((res) => {
            console.log('@@ res => ', res)
            if (res.data.message === 'Usuario registrado satisfactoriamente') {
              this.$emit('evento', {
                message: res.data.message,
                color: 'green',
                type: 'success'
              })
              this.showNuevo = false
            }
          })
          .catch((err) => {
            console.log('@@@ err => ', err)
          })
      } else {
        alert('Faltan Datos')
      }
    }
  }
}
</script>
