<script setup>
// Import BlogPost component
import blogPost from './subcomponents/BlogPost2.vue'
import axios from 'axios'
</script>

<script>
export default {
  data() {
    return {
      posts: [] // array of post objects
    }  
  },
  computed: {
    baseUrl() {
      if (window.location.hostname == 'localhost')
        return 'http://localhost:3000'
      else {
        const codespace_host = window.location.hostname.replace('5173', '3000')
        return `https://${codespace_host}`;
      }
    }
  },
  created() { // created is a hook that executes as soon as Vue instance is created
    axios.get(`${this.baseUrl}/posts`)
      .then(response => {
        // this gets the data, which is an array
        this.posts = response.data
        console.log(response.data)
      })
      .catch(error => {
        this.posts = [{ entry: 'There was an error: ' + error.message }]
      })
  },
  methods: {
    deletePost(id) {
      // Send delete request to backend
      axios.get(`${this.baseUrl}/deletePost?id=${id}`)
        .then(response => {
          console.log('Post deleted:', response.data)
          // Remove the post from the local array
          this.posts = this.posts.filter(post => post.id !== id)
        })
        .catch(error => {
          console.error('Error deleting post:', error)
          alert('Failed to delete post: ' + error.message)
        })
    }
  }
}
</script>

<template>
  <div class="ex3">
    <h2>Blog Posts</h2>
    <div class="posts">
      <blogPost
        v-for="post in posts"
        :key="post.id"
        :subject="post.subject"
        :entry="post.entry"
        :mood="post.mood"
      >
        <button 
          @click="deletePost(post.id)" 
          class="btn btn-danger btn-sm"
        >
          Delete
        </button>
      </blogPost>
    </div>
  </div>
</template>

<style scoped>
.ex3 {
  max-width: 860px;
  margin: 14px auto;
  padding: 8px;
}
.posts {
  display: grid;
  gap: 12px;
  margin-top: 12px;
}
</style>