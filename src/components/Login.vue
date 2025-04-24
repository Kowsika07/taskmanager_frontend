<template>
    <div class = "row" align="center">
        <div xlass="col-sm-4">
            <h2 align="center"> Login </h2>

            <form @submit.prevent="LoginData" style="width:50%;">
                <div class="form-group" align="center">
                    <label>Email</label>
                    <input type="email" v-model="user.email" class="form-control" placeholder="Email" align="center">
                </div>

                <div class="form-group" align="center">
                    <label>Password</label>
                    <input type="password" v-model="user.password" class="form-control" placeholder="password" align="center">
                </div>
                <br>
                <button type="submit" class="btn btn-primary">Login</button>
            </form>
        </div>
    </div>
</template>
<script>
import Vue from 'vue'
import axios from 'axios'
Vue.use(axios)

export default {
  name: 'Register',
  data () {
    return {
      result: {},
      user: {
        firstname: '',
        lastname: '',
        mobile: '',
        email: '',
        password: ''
      }
    }
  },
  mounted () {
    console.log('mounted called...')
  },
  methods: {
    LoginData () {
      axios.post('http://127.0.0.1:8000/api/login', this.user)
        .then(
          ({data}) => {
            console.log(data)
            try {
              if (data.status === true) {
                alert('Logged In Successfully')
                localStorage.setItem('token', data.token)
                axios.defaults.headers.common['Authorization'] = `Bearer ${data.token}`
                alert(data.token)
                this.$router.push({ name: 'TaskList' }) // <-- redirect here
              } else {
                alert('Login Failed')
              }
            } catch (err) {
              alert('failed')
            }
          }
        )
    },
    created () {
      const token = localStorage.getItem('token')
      if (token) {
        alert(token)
        axios.defaults.headers.common['Authorization'] = `Bearer ${token}`
      }
    }
  }
}
</script>
