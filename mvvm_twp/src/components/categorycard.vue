<template>
  <div class="CategoryCard">
    <div class="category">
      {{ category.name }}
      <div class="icon">
        <button
          style="background-color: #ffe4e1"
          @click="deleteCategory(category.id)"
        >
          Delete
        </button>
      </div>
    </div>
    <draggable :list="post" group="posts">
      <postcard
        v-for="item in listofPosts"
        :key="item.id"
        :post="item"
        :id="item.id"
        @deletePost="deletePost"
      />
    </draggable>
    <formposts @addPosts="addPost" />
  </div>
</template>

<script>
import postcard from "@/components/postcard.vue";
import formposts from "@/components/formposts.vue";
import { VueDraggableNext } from "vue-draggable-next";
import WPAPI from "wpapi";
var wp = new WPAPI({
  endpoint: "http://localhost/blog/index.php/wp-json",
  username: "saber",
  password: "LyFG%lS!Ez!)A*v0wO7zQG8l",
  auth: true,
});
export default {
  name: "categorycard",
  props: {
    category: Object,
  },

  data: function () {
    return {
      listofPosts: [],
    };
  },
  created() {
    this.getAllPosts();
  },

  methods: {
    getAllPosts() {
      wp.posts()
        .categories(this.category.id)
        .then((data) => {
          this.listofPosts = data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    deleteCategory(id) {
      this.$emit("deleteCategory", id);
    },
    addPost(post) {
      //API for the cards
      wp.posts()
        .categories(this.category.id)
        .create({
          title: `${post.title}`,
          status: "publish",
          categories: [this.category.id],
        })
        .then((postFromApi) => {
          this.listofPosts.unshift(postFromApi);
        })
        .catch((error) => {
          console.log(error.message);
        });
    },
    deletePost(id) {
      wp.posts()
        .id(id)
        .delete({ force: true })
        .then(() => {
          const specificId = (post) => {
            console.log(post);
            return this.id == post.id;
          };
          const index = this.listofPosts.findIndex(specificId);
          this.listofPosts.splice(index, 1);
        });
    },
    updatePost(post) {
      //API for the cards
      wp.updatePost({
        title: `${post.title}`,
        status: "publish",
        categories: [this.category.id],
      })
        .then((postFromApi) => {
          this.listofPosts.unshift(postFromApi);
        })
        .catch((error) => {
          console.log(error.message);
        });
    },
  },
  components: {
    formposts,
    postcard,
    draggable: VueDraggableNext,
  },
};
</script>

<style scoped>
.CategoryCard {
  max-width: 100%;
  border: 5px solid grey;
  width: 350px;
  height: 150px;
  margin: 20px 20px auto;
  border-radius: 20px;
  box-shadow: -10px 10px 5px rgb(246, 247, 163);
  overflow-wrap: break-word;
  overflow-y: auto;
  display: inline-block;
  background-color: #ebecf0;
}

.category {
  display: inline-flex;
  font-size: 1.2em;
  font-weight: bolder;
}
#deletebutton {
  margin-left: 5px;
}
.category {
  display: inline-flex;
  font-size: 20px;
  font-weight: bold;
}
</style>