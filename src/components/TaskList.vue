<template>
    <div align="">

        <h1>Task Add and Update</h1>

        <form @submit.prevent="save" style="width:50%;" class="text-center mx-auto">
            <div class="form-group"  >
    <label for="exampleInputEmail1">Title</label>
    <input type="text" class="form-control" v-model="task.title" style="width:100%" name="title"  placeholder="Enter Title">
  </div>

  <div class="form-group">
    <label for="exampleInputPassword1">Description</label>
    <input type="text" class="form-control" v-model="task.description" style="width:100%" name="description" placeholder="Description">
  </div>

  <div class="form-group">
    <label for="exampleInputPassword1">Due Date</label>
    <input type="date" class="form-control" v-model="task.due_date" style="width:100%" name="due_date" placeholder="Due Date">
  </div>
<br>
  <input type="submit" value="save" class="btn btn-success">
</form>
<br>
        <h1>Task List</h1>
        <table class="table table-hover" align="center">
  <thead align="center">
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Title</th>
      <th scope="col">Due Date</th>
      <th scope="col">Status</th>
      <th scope="col">Action</th>
    </tr>
  </thead>
  <tbody align="center">
    <tr v-for="task in result" v-bind:key="task.id">
      <th scope="row">{{task.id}}</th>
      <td>{{task.title}}</td>
      <td>{{task.due_date}}</td>
<td>
  <button
    :class="task.status === 0 ? 'btn btn-warning' : 'btn btn-success'"
    @click="toggleStatus(task)"
  >
    {{ task.status === 0 ? 'Pending' : 'Completed' }}
  </button>
</td>      <td>
        <button type="button" class="btn btn-info" @click="edit(task)">Edit</button>
        <button type="button" class="btn btn-danger" @click="remove(task)">Delete</button>
      </td>
    </tr>
  </tbody>
</table>
    </div>
</template>
<script>
import Vue from 'vue'
import axios from 'axios'
Vue.use(axios)

export default {
  name: 'TaskList',
  data () {
    return {
      result: {},
      task: {
        title: '',
        description: '',
        due_date: ''
      }
    }
  },
  created () {
    this.TaskLoad()
  },
  methods: {
    TaskLoad () {
      var page = 'http://127.0.0.1:8000/api/tasklist'
      axios.get(page)
        .then(
          ({data}) => {
            console.log(data)
            this.result = data
          }
        )
    },
    save () {
      if (this.task.id === undefined) {
        this.saveTask()
      } else {
        this.updateData()
      }
    },
    saveTask () {
      axios.post('http://127.0.0.1:8000/api/store', this.task)
        .then(
          ({data}) => {
            console.log(data)
            if (data.status === true) {
              alert('Task added')
            } else {
              alert('Failed')
            }
          }
        )
    },
    edit (task) {
      this.task = task
    },
    updateData () {
      var edit = 'http://127.0.0.1:8000/api/update/' + this.task.id
      axios.put(edit, this.task)
        .then(
          ({data}) => {
            this.task.title = ''
            this.task.description = ''
            this.due_date = ''
            this.id = ''
            alert('updated')
            this.TaskLoad()
          }
        )
    },
    remove (task) {
      var url = 'http://127.0.0.1:8000/api/delete/' + task.id
      axios.delete(url)
      alert('deleted')
      this.TaskLoad()
    },
    toggleStatus (task) {
      axios.get('http://127.0.0.1:8000/api/status/' + task.id)
      alert('status updated')
    }
  }
}
</script>
