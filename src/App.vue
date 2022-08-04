<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <my-button
        @click="showDialog"
        style="margin: 15px 0;"
    >Создать пользователя
    </my-button>
    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>
    <post-list
        :posts="posts"
        @remove="removePost"
        v-if="!isPostsLoading"
    >
    </post-list>
    <div v-else>Идет загрузка...</div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyDialog from "./components/UI/MyDialog";
import MyButton from "./components/UI/MyButton";
import axios from "axios";

export default {
  components: {
    MyButton,
    MyDialog,
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    async fetchPosts() {
      this.isPostsLoading = true
      try {
        const response = await axios
            .get('https://jsonplaceholder.typicode.com/posts?_limit=10')
        this.posts = response.data
        this.isPostsLoading = false
      } catch (e) {
        alert('Error')
      }
    }
  },
  mounted() {
    this.fetchPosts()
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

form {
  display: flex;
  flex-direction: column;
}

.app {
  padding: 20px;
}

</style>