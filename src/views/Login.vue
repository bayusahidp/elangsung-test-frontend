<template>
  <v-app id="inspire">
    <v-content>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4>
            <v-card class="elevation-12">
              <v-toolbar dark color="warning">
                <v-toolbar-title>Login</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field
                    v-model="username"
                    name="username"
                    label="Username"
                    type="text"
                  ></v-text-field>
                  <v-text-field
                    v-model="password"
                    id="password"
                    name="password"
                    label="Password"
                    type="password"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-row justify="center" class="pb-4">
                <v-card-actions>
                  <v-btn v-on:click="login" color="warning" to="/">Login</v-btn>
                </v-card-actions>
              </v-row>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import axios from "axios"
export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
    }
  },
  methods: {
    login() {
      let self = this;
      axios({
        method: 'post',
        url: 'http://localhost:8000/api/auth/login-admin',
        data: {
          username: this.username,
          password: this.password,
        }
      }).then(function (response) {
        if (response.status == 200) {
          if (response.data.status == true) {
            localStorage.setItem('token', response.data.data.token);
            localStorage.setItem('username', response.data.data.user.username);
            self.$router.push({
              name: 'Dashboard'
            })
          }
          else {
            alert(response.data.message);
          }
        }
      }).catch(function (err) {
        alert('error');
      });
    }
  }
}

</script>