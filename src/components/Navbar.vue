<template>
    <v-card class="overflow-hidden">
    <v-app-bar
      absolute
      color="white"
      elevate-on-scroll
      scroll-target="#scrolling-techniques-7"
    >

      <v-toolbar-title>eLangsung Tes</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-p></v-p>

      <v-btn icon>
        <v-icon
        @click="logout()"
        >
        mdi-power
        </v-icon>
      </v-btn>
    </v-app-bar>
    <v-sheet
      id="scrolling-techniques-7"
      class="overflow-y-auto"
      max-height="600"
    >
      <v-container style="height: 60px;">
      </v-container>
    </v-sheet>
  </v-card>
</template>

<script>
export default {
  methods: {
    logout() {
      let self = this;
      var token = localStorage.getItem("token");
      axios({
        method: 'post',
        url: 'http://localhost:8000/api/auth/logout',
        headers: {
          "Authorization": "Bearer " + token
        }
      }).then((response) => {
        if (response.status == 200) {
          if (response.data.status == true) {
            alert(response.data.message);
            self.$router.push({
              name: 'Login'
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