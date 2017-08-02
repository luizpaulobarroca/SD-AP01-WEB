<template>
  <div>
    <div class="">
      <div class="input-group">
        <input type="text" class="form-control" v-model="name" placeholder="Name">
        <span class="input-group-btn">
          <button class="btn btn-default" type="button" v-on:click="create()">Create</button>
        </span>
      </div><!-- /input-group -->
    </div>
    <table class="table">
      <thead>
      <tr>
        <th>#</th>
        <th>Name</th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="uni in data">
        <td>{{uni.id}}</td>
        <td>{{uni.name}}</td>
        <td v-on:click="openModal(uni)">
          <a>
            <i class="glyphicon glyphicon-pencil"></i>
          </a>
        </td>
        <td v-on:click="deletee(uni.id)">
          <a>
            <i class="glyphicon glyphicon-remove-circle"></i>
          </a>
        </td>
      </tr>
      </tbody>
    </table>
    <modal name="edit" height="100">
      <div class="input-group">
        <input type="text" class="form-control" v-model="name" placeholder="Name">
        <span class="input-group-btn">
          <button class="btn btn-default" type="button" v-on:click="update()">Edit</button>
        </span>
      </div>
    </modal>
  </div>
</template>

<script>
import Vue from 'vue'
import VResource from 'vue-resource'

import VModal from 'vue-js-modal'
Vue.use(VResource)
Vue.use(VModal)
import 'bootstrap/dist/css/bootstrap.css'
const URL = 'http://localhost:9090/university'
export default {
  name: 'hello',
  data () {
    return {
      data: [],
      name: '',
      selected: null
    }
  },
  methods: {
    openModal (obj) {
      this.name = obj.name
      this.selected = obj.id
      this.$modal.show('edit')
    },
    create () {
      Vue.http.post(URL, {name: this.name}).then(response => {
        this.name = ''
        this.list()
      })
    },
    list () {
      Vue.http.get(URL).then(response => {
        this.data = response.data
      })
    },
    update () {
      Vue.http.put(URL + '/' + this.selected, {name: this.name}).then(response => {
        this.name = ''
        this.list()
        this.$modal.show('hide')
      })
    },
    deletee (id) {
      Vue.http.delete(URL + '/' + id, {name: this.name}).then(response => {
        this.list()
      })
    },
    confirm () {
      this.showSourceDialog = false
    }
  },
  mounted: function () {
    this.list()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
