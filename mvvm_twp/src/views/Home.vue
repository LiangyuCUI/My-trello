<template>
  <div class="home">
    <div class="header">
      <h1>YOUR TRELLO</h1>
    </div>
    <br />
    <div class="body">
      <br />
      <div class="categorycontainer">
        <div class="categorycontainer__board">
          <formcategories @addCategories="addCategories" id="addcategory" />

          <categorycard
            v-for="item in listOfCategories"
            :key="item.id"
            :category="item"
            :id="item.id"
            @deleteCategory="deleteCategory"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import formcategories from "@/components/formcategories.vue";
import categorycard from "@/components/categorycard.vue";
import WPAPI from "wpapi";
// import draggable from "vuedraggable";
var wp = new WPAPI({
  endpoint: "http://localhost/blog/index.php/wp-json",
  username: "saber",
  password: "LyFG%lS!Ez!)A*v0wO7zQG8l",
  auth: true,
});
export default {
  name: "Home",

  data: function () {
    return {
      listOfCategories: [],
    };
  },
  created() {
    this.getAll();
  },
  methods: {
    getAll() {
      wp.categories()
        .then((data) => {
          this.listOfCategories = data.filter(
            (category) => category.name != "Uncategorized"
          );
        })
        .catch((error) => {
          console.log(error);
        });
    },
    addCategories(category) {
      wp.categories()
        .create({
          name: `${category.name}`,
          // status: "publish"
        })
        .then((categoryFromApi) => {
          this.listOfCategories.unshift(categoryFromApi);
        })
        .catch((error) => {
          console.log(error.message);
        });
    },
    deleteCategory(id) {
      wp.categories()
        .id(id)
        .delete({ force: true })
        .then(() => {
          const specificId = (category) => {
            console.log(category);
            return id == category.id;
          };
          const index = this.listOfCategories.findIndex(specificId);
          this.listOfCategories.splice(index, 1);
        });
    },
    startDrag(event, category) {
      console.log(`start dragging category: ${category.name}`);
    },
  },
  components: {
    formcategories,
    categorycard,
  },
};
</script>
<style scoped>
.header {
  align-items: left !important;
  display: block !important;
  margin: 20px solid black !important;
}
.categorycontainer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  grid-auto-flow: column;
  grid-gap: 8px;
  overflow: auto;
  margin-left: 25px;
}
h1 {
  text-align: center;
}
h2 {
  text-align: center;
}
#addcategory {
  margin-left: 50px;
}
.categorycontainer {
  display: inline-block;
}

::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px grey;
  border-radius: 10px 70px;
}
::-webkit-scrollbar-thumb {
  background: grey;
  border-radius: 10px;
}
</style>