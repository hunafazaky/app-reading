<template>
  <div>
    <LoadingPage :loading="loading"/>
    <h1 class="text-center ma-4">Reading App</h1>
    <v-row justify="center" align="center">
      <v-col md="6" lg="5" v-if="height >= 500">
        <v-img src="/login-image.png" width="100%"></v-img>
      </v-col>
      <v-col cols="12" sm="8" md="4" lg="4">
        <v-card class="mb-2" shaped outlined elevation="5">
          <v-card-title v-text="form_title"></v-card-title>
          <v-card-text>
            <v-text-field
              dense
              v-model="user.username"
              label="Username"
              type="text"
              required
            ></v-text-field>
            <v-text-field
              dense
              v-model="user.password"
              label="Password"
              type="password"
              required
            ></v-text-field>
          </v-card-text>
          <v-card-actions class="mx-2">
            <v-btn
              v-if="form_title == 'Registrasi'"
              small
              color="success"
              @click="regis"
            >
              registrasi
            </v-btn>
            <v-btn
              v-if="form_title == 'Login'"
              small
              color="primary"
              @click="login"
            >
              login
            </v-btn>
          </v-card-actions>
        </v-card>
        <v-alert
          class="mb-0"
          type="info"
          transition="slide-y-transition"
          :value="regisAttempt || loginAttempt"
        >
          {{ message }}
        </v-alert>
        <span v-if="form_title == 'Registrasi'" class="caption text--secondary"
          >Sudah punya akun?
          <span class="text-decoration-none" @click.prevent="openLoginForm"
            >Login</span
          >
        </span>
        <span v-if="form_title == 'Login'" class="caption text--secondary"
          >Belum punya akun?
          <span
            class="text-decoration-none success--text"
            @click.prevent="openRegisForm"
            >Registrasi</span
          >
        </span>
      </v-col>
    </v-row>
  </div>
</template>

<style scoped>
/* .card {
  opacity: 90% !important;
} */
</style>

<script>
import LoadingComponent from '../components/LoadingComponent.vue'
import LoadingPage from '../components/LoadingPage.vue'

export default {
  layout: 'login',
  data: () => ({
    user: {
        username: null,
        password: null,
    },
    loginAttempt: false,
    regisAttempt: false,
    form_title: 'Registrasi',
    message: '',
    loading: false,
  }),
  components: {
    LoadingComponent,
    LoadingPage,
  },
  computed: {
    height() {
      switch (this.$vuetify.breakpoint.name) {
        case 'xs':
          return 220
        case 'sm':
          return 400
        case 'md':
          return 500
        case 'lg':
          return 600
        case 'xl':
          return 800
      }
    },
  },

  methods: {
    openLoginForm() {
      this.form_title = 'Login'
    },
    openRegisForm() {
      this.form_title = 'Registrasi'
    },
    login() {
      this.loading = true;
      this.$store
      .dispatch('login', this.user)
      .then((data) => {
        // this.loading = false;
        if (data.id) {
          this.message = 'Login Berhasil!!';
          this.loginAttempt = true;
          this.$router.push('/home');
        } else {
          this.message = 'Error: ' + data.message;
          this.loginAttempt = true;
          this.loading = false;
        };
      })
      .catch((err) => {
        alert(err)
        this.loading = false;
      });
    },
    regis() {
      this.regisAttempt = true;
      this.loading = true;
      this.$store
      .dispatch('regis', this.user)
      .then((data) => {
        this.loading = false;
        if (data.id) {
          this.message = 'Registrasi Berhasil!!';
          this.login();
        } else this.message = 'Error: ' + data.message;
      })
      .catch((err) => alert(err));
    },
  },
}
</script>
