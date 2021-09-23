<template>
  <div class="form">
    <form @submit.prevent="commit">
      <label for="categoryTitle">Category title:</label><br />
      <input
        type="text"
        id="categoryTitle"
        v-model="newPost.name"
        placeholder="Enter list title..."
      />
      <button type="submit" style="background-color: #ffb6c1">add list</button>
      <span class="clearButton" type="button" @click="clear()">&#10060;</span>
    </form>
  </div>
</template>

<script>
export default {
  name: "formcategories",
  props: {},
  data() {
    return {
      newPost: {
        name: "",
      },
    };
  },
  methods: {
    commit() {
      this.$emit("addCategories", { ...this.newPost });
      // return (this.newPost);
      this.clear();
    },
    clear() {
      this.newPost.name = "";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form {
  align-items: left;
}
</style>
