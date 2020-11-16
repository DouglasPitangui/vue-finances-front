<template>
  <v-container fill-height>
    <v-layout
      justify-center
      align-center
    >

      <v-flex
        xs12
        sm6
        md4
        lg3
        xl3
      >
        <v-card class="elevation-12">

          <v-toolbar
            color="primary"
            dark
          >
            <v-toolbar-title>{{ texts.toobar }}</v-toolbar-title>
          </v-toolbar>

          <v-card-text>
            <v-form>
              <v-text-field
                v-if="!isLogin"
                prepend-icon="person"
                name="name"
                label="Nome"
                type="text"
                :error-messages="nameErros"
                :success="!$v.user.name.$invalid"
                v-model.trim="$v.user.name.$model"
              ></v-text-field>
              <v-text-field
                prepend-icon="email"
                name="email"
                label="Email"
                type="email"
                :error-messages="emailErros"
                :success="!$v.user.email.$invalid"
                v-model.trim="$v.user.email.$model"
              ></v-text-field>
              <v-text-field
                prepend-icon="lock"
                name="password"
                label="Senha"
                type="password"
                :error-messages="passwordErros"
                :success="!$v.user.password.$invalid"
                v-model.trim="$v.user.password.$model"
              ></v-text-field>
            </v-form>
            <v-btn
              block
              depressed
              color="secondary"
              @click="isLogin = !isLogin"
            >{{texts.button}}</v-btn>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              :disabled="$v.$invalid"
              color="primary"
              large
              @click="submit"
            >{{ texts.toobar }}</v-btn>
          </v-card-actions>

        </v-card>
      </v-flex>

    </v-layout>
  </v-container>
</template>

<script>

import { required, email, minLength } from 'vuelidate/lib/validators'

export default {
  name: 'Login',
  data: () => ({
    isLogin: true,
    user: {
      name: '',
      email: '',
      password: ''
    }
  }),
  validations () {
    const validations = {
      user: {
        email: {
          required,
          email
        },
        password: {
          required,
          minLength: minLength(6)
        }
      }
    }

    if (this.isLogin) { return validations }
    return {
      user: {
        ...validations.user,
        name: {
          required,
          minLength: minLength(3)
        }
      }
    }
  },
  computed: {
    texts () {
      return this.isLogin
        ? { toobar: 'Entrar', button: 'Criar conta' }
        : { toobar: 'Criar Conta', button: 'Já tenho um conta' }
    },
    nameErros () {
      const erros = []
      const name = this.$v.user.name
      // $dirty valida se o usuário já digitou algo no campo
      if (!name.$dirty) { return erros }
      // validação das propriedades do vuelidate required e minLength
      if (!name.required) { erros.push('Nome obrigatório') }
      if (!name.minLength) { erros.push(`Insira pelo menos ${name.$params.minLength.min} caracteres!`) }

      return erros
    },
    emailErros () {
      const erros = []
      const email = this.$v.user.email
      // $dirty valida se o usuário já digitou algo no campo
      if (!email.$dirty) { return erros }
      if (!email.email) { erros.push('Insira um email válido') }
      // validação das propriedades do vuelidate required e email
      if (!email.required) { erros.push('Email obrigatório') }
      if (!email.email) { erros.push('Insira um email válido') }

      return erros
    },
    passwordErros () {
      const erros = []
      const password = this.$v.user.password
      // $dirty valida se o usuário já digitou algo no campo
      if (!password.$dirty) { return erros }
      // validação das propriedades do vuelidate required e minLength
      if (!password.required) { erros.push('Senha obrigatório') }
      if (!password.minLength) { erros.push(`Insira pelo menos ${password.$params.minLength.min} caracteres!`) }

      return erros
    }
  },
  methods: {
    log () {
      console.log('Vuelidate', this.$v)
    },
    submit () {
      console.log('User: ', this.user)
    }
  }
}
</script>
