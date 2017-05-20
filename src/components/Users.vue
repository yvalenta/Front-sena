<template lang="html" id="users">
  <div>
    <div class="form" v-if="onCreate || onEdit">
      <h3 v-if="onCreate">Nuevo usuario</h3>
      <h3 v-if="onEdit">Editar usuario</h3>
      <div class="row">
        <div class="col-md-6">
          <label for="name">Nombre</label>
          <input type="text" name="name" v-model="user.name" class="form-control">
          <span style="color:red">{{errors.name}}</span>
        </div>
        <div class="col-md-6">
          <label for="name">Username</label>
          <input type="text" name="name" v-model="user.username" class="form-control">
          <span style="color:red">{{errors.username}}</span>
        </div>
        <div class="col-md-6">
          <label for="name">Email</label>
          <input type="mail" name="name" v-model="user.email" class="form-control">
          <span style="color:red">{{errors.email}}</span>
        </div>
        <div class="col-md-6">
          <label for="name">role</label>
          <select class="form-control" name="role" v-model="user.role">
            <option v-for="rol in roles" v-bind:value="rol" value="role">{{rol}}</option>
          </select>
          <span style="color:red">{{errors.role}}</span>
        </div>
        <div class="col-md-6">
          <label for="name">password</label>
          <input type="password" name="name" v-model="user.password" class="form-control">
          <span style="color:red">{{errors.password}}</span>
        </div>
        <div class="col-md-6">
          <a href="#" @click="createUser" class="btn btn-success" v-if="onCreate">Crear</a>
          <a href="#" @click="updateUser" class="btn btn-success" v-if="onEdit">Editar</a>
        </div>
      </div>
    </div>

    <div class="list" v-if="!onCreate && !onEdit">
      <div class="row">
        <div class="col-md-11">
          <h3>Users</h3>
        </div>
        <div class="col-md-2">
          <a href="#" @click="fetchAll" class="btn btn-success">Refresh</a>
          <a href="#" @click="onCreate=true" class="btn btn-primary">Crear usuario</a>

        </div>
      </div>

      <table class="table table-sm">
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Username</th>
          <th>Email</th>
          <th>Role</th>
          <th>actions</th>
        </tr>
        <tr v-for="(user, index) in users">
          <td>{{user.id}}</td>
          <td>{{user.name}}</td>
          <td>{{user.username}}</td>
          <td>{{user.email}}</td>
          <td>{{user.role}}</td>
          <td>
          <a href="#" @click="editUser(index)">Edit</a>
          <a href="#" @click="remove(user.id)">Delete</a>
          </td>
        </tr>
      </table>
      <a href="#" @click="onCreate=true">Crear usuario</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Users',
  data () {
    return {
      onCreate: false,
      onEdit: false,
      user: { username: '', name: '', role: 'customer', email: '', password: '' },
      users: [],
      errors: {},
      roles: ['customer', 'support', 'admin']
    }
  },
  methods: {
    fetchAll: function () {
      this.$http.get('users.json')
        .then(data => {
          this.users = data.body
        })
    },
    editUser: function (index) {
      this.onEdit = true
      this.user = this.users[index]
    },
    createUser: function () {
      this.$http.post('users.json', this.user)
        .then(response => {
          this.fetchAll()
          this.onCreate = false
          this.user = { username: '', name: '', role: '', email: '', password: '' }
        },
        response => {
          this.errors = response.body
        })
    },
    updateUser: function () {
      this.$http.patch('users/' + this.user.id + '.json', this.user)
        .then(response => {
          this.onEdit = false
          this.user = { username: '', name: '', role: '', email: '', password: '' }
        }, response => {
          console.log('response.body')
          this.errors = response.body
        })
    },
    remove: function (id) {
      this.$http.delete('users/' + id + '.json')
        .then(() => this.fetchAll())
    }
  },
  mounted: function () {
    this.fetchAll()
  }
}
</script>script>

<style lang="css">

</style>
