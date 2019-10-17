<template>
  <div class="app flex-row align-items-center">
    <div class="container">
      <b-row class="justify-content-center">
        <b-col md="8">
          <b-card-group>
            <b-card no-body class="p-4">
              <b-card-body>
                <b-form>
                  <h1>Login</h1>
                  <p class="text-muted">Sign In to your account</p>
                    <b-input-group class="mb-3">
                      <b-input-group-prepend><b-input-group-text><i class="icon-user"></i></b-input-group-text></b-input-group-prepend>
                      <b-form-input type="email" v-model="email" class="form-control" placeholder="Email" autocomplete="username email" />
                    </b-input-group>
                    <b-input-group class="mb-3">
                      <b-input-group-prepend><b-input-group-text><i class="icon-user"></i></b-input-group-text></b-input-group-prepend>
                      <b-form-input type="text" v-model="firstname" class="form-control" placeholder="Nombre" autocomplete="username firstname" />
                    </b-input-group>
                    <b-input-group class="mb-3">
                      <b-input-group-prepend><b-input-group-text><i class="icon-user"></i></b-input-group-text></b-input-group-prepend>
                      <b-form-input type="text" v-model="lastname" class="form-control" placeholder="Apellido" autocomplete="username lastname" />
                    </b-input-group>                  
                    <b-input-group class="mb-4">
                      <b-input-group-prepend><b-input-group-text><i class="icon-lock"></i></b-input-group-text></b-input-group-prepend>
                      <b-form-input type="password" v-model="password" class="form-control" placeholder="Password" autocomplete="current-password" />
                    </b-input-group>
                  <b-row>
                    <b-col cols="6">
                      <b-button variant="primary" class="px-4" @click="postLogin()">Login</b-button>
                    </b-col>
                    <b-col cols="6" class="text-right">
                      <b-button variant="link" class="px-0">Forgot password?</b-button>
                    </b-col>
                  </b-row>
                </b-form>
              </b-card-body>
            </b-card>
            <b-card no-body class="text-white bg-primary py-5 d-md-down-none" style="width:44%">
              <b-card-body class="text-center">
                <div>
                  <h2>Sign up</h2>
                  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
                  <b-button variant="primary" class="active mt-3">Register Now!</b-button>
                </div>
              </b-card-body>
            </b-card>
          </b-card-group>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
const Cookie = process.client ? require('js-cookie') : undefined

export default {
  name: 'Login',
  layout: 'clean',
  middleware: 'notAuthenticated',
  data () {
    return {
      email : "",
      firstname : "",
      lastname: "",
      password: ""
    }
  },  
  methods: {
    postLogin() {
      let email = this.email 
      let firstName = this.firstname 
      let lastName = this.lastname 
      let password = this.password
      this.$axios.$post('api/auth', { email, firstName, lastName, password })
      .then(res => {
        const auth = {
          accessToken: res.token
        }
        this.$store.commit('setAuth', auth)
        Cookie.set('auth', auth)
        this.$router.push('/')
      })
    }
  }
}
</script>
