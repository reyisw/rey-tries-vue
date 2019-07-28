<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <div class="text-xs-center">
        <logo />
      </div>
      <v-card>
        <v-card-title class="headline">Welcome, {{ name }}</v-card-title>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
import Logo from "~/components/Logo.vue";
export default {
  components: {
    Logo
  },
  data() {
    return {
      name: ""
    };
  },
  mounted() {
    this.fetchUserDetails();
  },
  methods: {
    /**
     * [fetchUserDetails hace un fetch de los detalles del usuario logeado]
     * @return {[type]} [none]
     */
    fetchUserDetails() {
      const token = sessionStorage.getItem("token");
      console.log(token);
      const URL = `https://hidden-depths-47488.herokuapp.com/api/user?token=${token}`;

      if (token === null) {
        this.$router.push("/login");
      }

      this.$axios({
        method: "get",
        url: URL,
        headers: {
          Accept: "application/json"
        }
      })
        .then(res => {
          this.name = res.data.user.name;
        })
        .catch(err => {
          throw new Error(err);
        });
    }
  }
};
</script>