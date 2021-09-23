
<template>
  <div class="PostCard">
    <div class="post">
      {{ post.title.rendered }}
      <div class="icon">
        <button @click="deletePost(post.id)">Delete Post</button>
        <button class="redirect-button" v-on:click="redirect()">details</button>
      </div>
    </div>
  </div>
</template>

<script>
// import WPAPI from "wpapi";
// var wp = new WPAPI({
//   endpoint: "http://localhost/blog/index.php/wp-json",
//   username: "saber",
//   password: "LyFG%lS!Ez!)A*v0wO7zQG8l",
//   auth: true,
// });

export default {
  name: "postcard",

  props: {
    post: [Object],
  },
  // suppress
  // add de la suppresion des posts en changeants le .categories présent dans le code
  // categorycard.
  // on a alors un deletepost qui va dans wordpress. ensuite post. et cherche tout
  // les id en utilisant (id) on lui permet de choisir l'id a supprimer.
  data: function () {
    return {
      postdetails: [],
      drag: false,
    };
  },

  methods: {
    deletePost(id) {
      this.$emit("deletePost", id);
    },
    redirect() {
      this.$router.push({
        name: "About",
        params: {
          id: this.post.id,
          title: this.post.title.rendered,
        },
      });
    },
  },
  // components: {
  //   draggable,
  // },
};
</script>

<style scoped>
.post {
  display: inline-flex;
}
.PostCard {
  border: 1px solid grey;

  background-color: whitesmoke;
  border-radius: 10px;
  margin: 5px;
  display: block;
}

button.icon {
  align-self: right;
}
</style>