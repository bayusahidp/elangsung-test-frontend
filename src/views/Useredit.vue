<template>
  <div>
    <navbar/>
    <v-container>
      <v-row justify="center">
        <v-col lg="6">
          <div v-bind:key="post">
            <h2>{{ post.name }}</h2>
            <v-container>
              <p>Foto</p>
              <v-img
                v-bind:src="post.foto"
                max-height="400"
              >
              </v-img>
              <v-form>
                <v-col>
                  <v-file-input
                    outlined
                    dense
                    accept="image/*"
                    v-model="foto"
                    label="Foto"
                    name="foto"
                  ></v-file-input>
                </v-col>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="post.name"
                      label="Name"
                      name="name"
                      type="text"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="post.username"
                      label="Username"
                      name="username"
                      type="text"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="post.email"
                      label="Email"
                      name="email"
                      type="email"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="post.fakultas"
                      label="Fakultas"
                      name="fakultas"
                      type="text"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="post.jurusan"
                      label="Jurusan"
                      name="jurusan"
                      type="text"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="post.alamat"
                      label="Alamat"
                      name="alamat"
                      type="text"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-form>
              <v-row justify="center" class="pb-4">
                  <v-card-actions>
                    <v-btn v-on:click="update" color="warning">Save</v-btn>
                  </v-card-actions>
                </v-row>
            </v-container>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
import Navbar from "../components/Navbar.vue";

export default {
components: { Navbar },

  data () {
    return {
      post: {},
      name: '',
      username: '',
      email: '',
      fakultas: '',
      jurusan: '',
      alamat: '',
    }
  },

  mounted: function () {
    let self = this;

    var token = localStorage.getItem("token");
    axios({
        method: "get",
        url: "http://localhost:8000/api/user/detail/" + this.$route.params.id,
        headers: {
            "Authorization": "Bearer " + token
        }
    }).then((response) => {
      if (response.status == 200) {
        if (response.data.status == true) {
            this.post = response.data.data;
        }
      }
    }).catch((err) => {
      alert('error');
      let self = this;
      this.$router.push({
        name: 'Login',
      })
    });
  },

  methods: {
    update() {
      console.log(this.foto);
      var token = localStorage.getItem("token");
      let self = this;
      let formData = new FormData()
      formData.append('name', this.post.name);
      formData.append('username', this.post.username);
      formData.append('email', this.post.email);
      formData.append('fakultas', this.post.fakultas);
      formData.append('jurusan', this.post.jurusan);
      formData.append('alamat', this.post.alamat);
      formData.append('foto', this.foto);
      axios({
        method: 'post',
        url: 'http://localhost:8000/api/user/update/' + this.$route.params.id,
        data: formData,
        headers: {
          "Content-Type": "multipart/form-data",
          "Authorization": "Bearer " + token
        }
      }).then((response) => {
        if (response.status == 200) {
          if (response.data.status == true) {
            alert(response.data.message);
            self.$router.push({
              name: 'Dashboard'
            })
          }
          else {
            alert(response.data.error);
          }
        }
      }).catch((err) => {
        alert('error');
        console.log(err);
        self.$router.push({
          name: 'Login',
        })
      });
    }
  }

}
</script>