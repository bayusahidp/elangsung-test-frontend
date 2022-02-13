<template>
<div>
  <navbar/>
  <v-container>
    <v-data-table
    :headers="headers"
    :items="users"
    :items-per-page="5"
    class="elevation-1"
  >
  <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>Data User</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
  </v-container>
</div>
</template>

<script>
import axios from "axios"
import Navbar from "../components/Navbar.vue";
export default {
  components: { Navbar },
    mounted: function () {
      var token = localStorage.getItem("token");
      axios({
          method: "get",
          url: "http://127.0.0.1:8000/api/user",
          headers: {
              "Authorization": "Bearer " + token
          }
      }).then((response) => {
          console.log(response.data.data);
          if (response.status == 200 || response.status == 400) {
              if (response.data.status == true) {
                  console.log(response.data);
                  this.users = response.data.data;
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
    data() {
        return {
            dialog: false,
            dialogDelete: false,
            headers: [
                {
                    align: "start",
                    sortable: false,
                },
                { text: "Nama", value: "name" },
                { text: "Username", value: "username" },
                { text: "Email", value: "email" },
                { text: "Fakultas", value: "fakultas" },
                { text: "Jurusan", value: "jurusan" },
                { text: "Alamat", value: "alamat" },
                { text: "Actions", value: "actions", sortable: false }
            ],
            users: [],
            editedIndex: -1,
            editedItem: {
                name: "",
                username: "",
                email: "",
                fakultas: "",
                jurusan: "",
                alamat: "",
            },
            defaultItem: {
                name: "",
                username: "",
                email: "",
                fakultas: "",
                jurusan: "",
                alamat: "",
            },
        };
    },
    created() {
        this.initialize();
    },
    methods: {
      editItem(item) {
          let self = this;
          self.$router.push({
            name: 'Useredit',
            params: { id: item.id }
          })
      },
      deleteItem(item) {
        if (confirm("Apakah anda yakin ingin menghapus data tersebut?") == true) {
          var token = localStorage.getItem("token");
          let self = this;
          axios({
            method: 'delete',
            url: 'http://localhost:8000/api/user/delete/'+item.id,
            headers: {
                "Authorization": "Bearer " + token
            }
          }).then((response) => {
            if (response.status == 200) {
              if (response.data.status == true) {
                alert(response.data.message);
                location.reload();
              }
              else {
                alert(response.data.message);
              }
            }
          }).catch((err) => {
            alert('error');
            let self = this;
            this.$router.push({
              name: 'Login',
            })
          });
        }
      },
    },
}
</script>