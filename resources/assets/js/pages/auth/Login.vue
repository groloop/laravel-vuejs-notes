<template>
  <div class="col-md-8 col-md-offset-2">
    <div class="page-header">
      <h2 class="text-center">Login</h2>
    </div>

    <form @submit.prevent="login" @keydown="form.errors.clear($event.target.name)">
      <alert-error :form="form" v-if="form.errors.has('error')"></alert-error>

      <div class="form-group row" :class="{ 'has-error': form.errors.has('email') }">
        <label for="email" class="col-sm-3 col-form-label text-sm-right">Email</label>
        <div class="col-sm-9">
          <input v-model="form.email" type="text" name="email" id="email" class="form-control">
          <has-error :form="form" field="email"></has-error>
        </div>
      </div>

      <div class="form-group row" :class="{ 'has-error': form.errors.has('password') }">
        <label for="password" class="col-sm-3 col-form-label text-sm-right">Password</label>
        <div class="col-sm-9">
          <input v-model="form.password" type="password" name="password" id="password" class="form-control">
          <has-error :form="form" field="password"></has-error>
        </div>
      </div>

      <div class="form-group row">
        <label class="col-md-4 control-label"></label>
        <div class="col-md-4 center-block">
          <button :disabled="form.busy" type="submit" class="btn btn-block btn-primary">Login</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import Form from 'vform'

export default {
  name: 'login',

  data: () => ({
    form: new Form({
      email: '',
      password: ''
    })
  }),

  methods: {
    login() {
      this.form.post('/api/login')
      .then(({ data }) => {
        // console.log(data)
        this.$store.dispatch('saveToken', data.token)
        this.$store.dispatch('setCurrentUser', data.user)
        this.$router.push({ name: 'home' })
      }).catch((error) => {
        // console.log(error)
      })
    }
  }
}
</script>
