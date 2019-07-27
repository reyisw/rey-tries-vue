<template>
  <v-layout column justify-center align-center>
    <v-data-table :headers="headers" :items="listData" class="elevation-0">
      <template slot="items" slot-scope="props">
        <tr>
          <td>{{ props.item.id }}</td>
          <td>{{ props.item.article_title }}</td>
          <td>{{ props.item.article_description }}</td>
          <td>
            <v-btn color="error">DELETE</v-btn>
            <v-btn color="primary">EDIT</v-btn>
          </td>
        </tr>
      </template>
    </v-data-table>
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
    fetchArticles() {
      const token = sessionStorage.getItem("token");
      const URL = "https://hidden-depths-47488.herokuapp.com/api/article";

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
    }
  }
};
</script>
