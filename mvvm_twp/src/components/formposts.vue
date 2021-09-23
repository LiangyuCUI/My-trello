<template>
  <div class="form">
    <form @submit.prevent="createPost">
      <input
        type="text"
        id="postContent"
        v-model="newPost.title"
        placeholder="Enter the title of this post (to do...)"
      />
      <button type="submit">&#10133;</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "formposts",
  props: {
    category: Object,
  },
  data() {
    return {
      newPost: {
        title: "",
      },
    };
  },
  methods: {
    createPost() {
      this.$emit("addPosts", { ...this.newPost });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>