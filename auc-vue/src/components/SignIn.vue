<template>
  <div class="container mt-5">
    <div class="col-md-6 offset-md-3">
      <div class="card">
        <div class="card-body">
          <h2 class="card-title text-center">Login</h2>
          <form @submit.prevent="login">
            <div class="form-group mb-3">
              <label for="email">Email:</label>
              <input type="email" v-model.lazy="email" class="form-control" :class="{'border-danger': wrongLogin}"
                     id="email" required autocomplete="email">
            </div>
            <div class="form-group mb-3">
              <label for="password">Password:</label>
              <input type="password" v-model.lazy="password" class="form-control" :class="{'border-danger': wrongLogin}"
                     id="password" required autocomplete="current-password">
            </div>
            <div class="d-flex align-items-start justify-content-between">
              <p :class="{'text-danger': wrongLogin}" class="w-75">{{ message }}</p>
              <button type="submit" class="btn btn-primary btn-block">Login</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SignIn",
  inject: ["sessionService"],
  data() {
    return {
      email: '',
      password: '',
      message: '',
      wrongLogin: false
    };
  },
  methods: {
    async login() {
      try {
        await this.sessionService.asyncSignIn(this.email, this.password);
        this.wrongLogin = false
        this.message = this.sessionService.currentToken
      } catch (e) {
        this.wrongLogin = true
        this.message = "wrong credentials. Please fill in the right email and password!"
      }
    },
    //handle logging out, clear all imputs and push route back to route without query param
    handleLogout() {
      if(this.$route.query.signOf) {
        this.sessionService.signOut()
        this.email = ""
        this.password = ""
        this.message = ""
        this.$router.push({path: "/sign-in"})
      }
    }

  },
  created() {
    this.handleLogout()
  },
  watch: {
    //watch if query changes in login page
    '$route.query'() {
      this.handleLogout()
    }
  }
}
</script>


<style scoped>

</style>