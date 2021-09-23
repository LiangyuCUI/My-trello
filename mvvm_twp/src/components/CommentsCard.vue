<template>
  <div id="CommentCard">
    <div>
      <span>
        <h6 class="CommentCard-header">{{ comment.content.rendered }}</h6>
      </span>
      <button @click="deleteComment(comment.id)">delete</button>
    </div>
  </div>
</template>

<script>
import WPAPI from "wpapi";
var wp = new WPAPI({
  endpoint: "http://localhost/blog/index.php/wp-json",
  username: "saber",
  password: "LyFG%lS!Ez!)A*v0wO7zQG8l",
  auth: true,
});
export default {
name: "CommentsCard",
props: {
comment: Object,
},
methods: {
deleteComment(id) {
wp.comments().id(id).delete({ force: true });
this.$emit("deleteCom", id);
},
},
};
</script>

