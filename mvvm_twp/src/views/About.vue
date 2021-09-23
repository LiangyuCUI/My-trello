<template>
  <div class="details">
    <button class="btn btn-warning hBack" @click="$router.back()">Back</button>
    <br />
    <!-- <h1>{{$route.params.id}}</h1> -->
    <h1>{{ $route.params.title }}</h1>
    <!-- <h2>{{ $route.params.id }}</h2> -->
  </div>
  <div>
    <create-comments @create-new-comment="addcomments" />

    <comments-card
      v-for="(item, index) in listofcomments"
      :key="index"
      :comment="item"
      @deleteCom="deleteCom"
    />
  </div>
</template>
<script>
import CommentsCard from "@/components/CommentsCard.vue";
import createComments from "@/components/createComments.vue";

import WPAPI from "wpapi";
var wp = new WPAPI({
  endpoint: "http://localhost/blog/index.php/wp-json",
  username: "saber",
  password: "LyFG%lS!Ez!)A*v0wO7zQG8l",
  auth: true,
});

export default {
  name: "comments",
  data: function () {
    return {
      listofcomments: [],
    };
  },

  created() {
    this.getAllcomments();
  },

  methods: {
    getAllcomments() {
      wp.comments()
        .param("post", this.$route.params.id)
        .then((response) => {
          this.listofcomments = response;
        });
      // wp.comments(this.$route.params.id)
      // .then((data) => {
      // this.listofcomments = data
      // // .filter(
      // // (com) => com.post = this.$route.params.id)
      // })
      // .catch((error) => {
      // console.log(error);
      // });
    },
    addcomments(comment) {
      wp.comments()
        .create({
          content: `${comment.content}`,
          post: this.$route.params.id,
        })

        .then((data) => {
          this.listofPosts = data;
        });
    },
    deleteCom(id) {
      this.listofcomments = this.listofcomments.filter((n) => n.id !== id);
    },
  },
  components: {
    CommentsCard,
    createComments,
  },
};
</script>
