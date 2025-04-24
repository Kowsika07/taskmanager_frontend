<template>
    <div class="card" align="left">
        <div class="card-header" align="center">Register Form</div>
        <div class="card-body" align="center">

            <form @submit.prevent="saveData" style="width:50%;" >

                <label>First name</label>
                <input type="text" v-model="user.firstname" name="firstname" id="firstname" class="form-control" />

                <label>Last Name</label>
                <input type="text" v-model="user.lastname" name="firstname" id="lastname" class="form-control"/>

                <label>Mobile</label>
                <input type="text" v-model="user.mobile" name="firstname" id="mobile" class="form-control"/>

                <label>Email</label>
                <input type="text" v-model="user.email" name="firstname" id="email" class="form-control"/>

                <label>Password</label>
                <input type="text" v-model="user.password" name="password" id="password" class="form-control"/>

                <input type="submit" value="save" class="btn btn-success">
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
  created () {
  },
  mounted () {
    console.log('mounted called...')
  },
  methods: {
    saveData () {
      axios.post('http://127.0.0.1:8000/api/register', this.user)
        .then(
          ({data}) => {
            console.log(data)
            try {
              alert('saved')
            } catch (err) {
              alert('failed')
            }
          }
        )
    }
  }
}
</script>
