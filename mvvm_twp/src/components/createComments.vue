<template>
<div class="Create-Comment">
<h3>Comments</h3>
<form @submit.prevent="createComments">
      <input
      class="form-control-lg"
        type="text"
        id="commentContent"
        v-model="newComment.content"
      />
      <button type="submit"  class="btn btn-success">Add comment &#10133;</button>
    </form>
    </div>
</template>
<script>
export default {
  name: "Create-Comment",
  data: function () {
    return {
      newComment: {
        content:"",
      },
    };
  },
  methods: {
    createComments() {
      this.$emit("create-new-comment", { ...this.newComment });
      this.newComment = {};
    },
  },
};
</script>