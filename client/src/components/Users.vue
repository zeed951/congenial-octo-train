<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-10">
        <h1>Users</h1>
        <hr>
        <div>
            <b-alert dismissible v-model="showMessage" fade variant="success">{{message}}</b-alert>
            <b-alert dismissible v-model="showErrorMessage" fade variant="danger">{{errorMessage}}</b-alert>
        </div>
        <br><br>
        <table class="table table-hover">
          <thead>
            <tr>
              <th scope="col">id</th>
              <th scope="col">username</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(user, index) in users" :key="index">
              <td>{{ user.id }}</td>
              <td>{{ user.username }}</td>
              <td>
                <div class="btn-group" role="group">
                  <button
                          type="button"
                          class="btn btn-warning btn-sm"
                          v-b-modal.book-update-modal
                          @click="on_redir_to_user(user)">
                      Login
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Alert from './Alert.vue';

export default {
  data() {
    return {
      users: [],
      message: '',
      showMessage: false,
      errorMessage: '',
      showErrorMessage: false,
      // addBookForm: {
      //   title: '',
      //   author: '',
      //   read: [],
      // },
      // message: '',
      // showMessage: false,
      // editForm: {
      //   id: '',
      //   title: '',
      //   author: '',
      //   read: [],
      // },
    };
  },
  components: {
    alert: Alert,
  },
  methods: {
    getUsers() {
      const path = window.location.protocol + "//" + window.location.hostname + ':' + process.env.VUE_APP_GW_PORT + '/user';
      console.log(path)
      axios.get(path)
        .then((res) => {
          this.users = res.data.users;
          console.log(this.users);
        })
        .catch((error) => {
          this.errorMessage = error.response.data.message
          this.showErrorMessage = true;
          console.error(error);
        });
    },
    on_redir_to_user(user) {
      this.$router.push(`/user/${user.id}/order`);      
    },
  },
  created() {
    this.getUsers();
  },
};
</script>
