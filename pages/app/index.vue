<template>
  <v-layout column justify-center align-center>
    <v-data-table :headers="headers" :items="listData" class="elevation-0">
      <template slot="items" slot-scope="props">
        <tr>
          <td>{{ props.item.id }}</td>
          <td>{{ props.item.article_title }}</td>
          <td>{{ props.item.article_description }}</td>
          <td>
            <v-btn color="error" @click="deleteArticle(props.item.id)">Eliminar</v-btn>
            <v-btn color="primary" :to="`/app/edit/${props.item.id}`">Editar</v-btn>
          </td>
        </tr>
      </template>
    </v-data-table>
    <v-btn color="success" :to="`/app/create`">Nuevo articulo</v-btn>
  </v-layout>
</template>

<script>
export default {
  data() {
    return {
      listData: [],
      headers: [
        {
          text: "Id",
          value: "id"
        },
        {
          text: "Nombre del articulo",
          value: "article_title"
        },
        { text: "Descripción", value: "article_description" },
        { text: "Action", value: "action" }
      ]
    };
  },
  mounted() {
    this.fetchArticles();
  },
  methods: {
    /**
     * [fetchArticles hace un fetch de los articulos]
     */
    fetchArticles() {
      const token = sessionStorage.getItem("token");
      const URL = "https://hidden-depths-47488.herokuapp.com/api/article";

      if (token === null) {
        this.$router.push("/login");
      }

      this.$axios({
        method: "get",
        url: URL,
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`
        }
      })
        .then(res => {
          this.listData = res.data.data;
        })
        .catch(err => {
          throw new Error(err);
        });
    },
    /**
     * [deleteArticle hace un fetch de los articulos]
     */
    deleteArticle(id) {
      const token = sessionStorage.getItem("token");
      const URL = `https://hidden-depths-47488.herokuapp.com/api/article/${id}`;

      this.$axios({
        method: "delete",
        url: URL,
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${token}`
        }
      })
        .then(_ => {
          this.fetchArticles();
        })
        .catch(err => {
          throw new Error(err);
        });
    }
  }
};
</script>
