<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <v-card width="400px">
        <v-card-title class="headline">Actualizar articulo</v-card-title>
        <v-card-text>
          <form @submit.prevent="updateArticle(id)">
            <v-text-field v-model="article_title" label="Nombre del articulo" required></v-text-field>
            <v-text-field v-model="article_description" label="Descripción" required></v-text-field>
            <v-btn type="submit">Actualizar</v-btn>
          </form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  data() {
    return {
      id: "",
      article_title: "",
      article_description: ""
    };
  },
  mounted() {
    this.id = this.$route.params.id;
    this.fetchArticle(this.id);
  },
  methods: {
    /**
     * hace un fetch de un articulo para actualizar sus campos
     * @return {[type]} [none]
     */
    fetchArticle(id) {
      const token = sessionStorage.getItem("token");
      const URL = `https://hidden-depths-47488.herokuapp.com/api/article/${id}`;

      this.$axios({
        method: "get",
        url: URL,
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`
        }
      })
        .then(res => {
          const { article_title, article_description } = res.data.data;
          this.article_title = article_title;
          this.article_description = article_description;
        })
        .catch(err => {
          throw new Error(err);
        });
    },
    /**
     * [updateArticle actualizar un articulo]
     */
    updateArticle(id) {
      const { article_title, article_description } = this;
      const data = { article_title, article_description };
      const token = sessionStorage.getItem("token");
      const URL = `https://hidden-depths-47488.herokuapp.com/api/article/${id}`;

      this.$axios({
        method: "put",
        url: URL,
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`
        },
        data: data
      })
        .then(res => {
          this.$router.push("/app");
        })
        .catch(err => {
          throw new Eror(err);
        });
    }
  }
};
</script>
