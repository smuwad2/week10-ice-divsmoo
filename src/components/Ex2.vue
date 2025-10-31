<template>
  <div class="ex2">
    <h2>Blog Posts</h2>
    <div v-if="loading">Loading posts...</div>
    <div v-else-if="error" class="error">{{ error }}</div>
    <div v-else class="posts">
      <BlogPost
        v-for="p in posts"
        :key="p.id ?? p._id ?? p.entry"
        :post="p"
      />
    </div>
  </div>
</template>

<script>
import BlogPost from "./subcomponents/BlogPost.vue";
import axios from "axios";

export default {
  name: "Ex2",
  components: { BlogPost },
  data() { 
    return { 
      posts: [], 
      loading: true, 
      error: "" 
    }; 
  },
  computed: {
    baseUrl() {
      if (window.location.hostname === "localhost") return "http://localhost:3000";
      const codespace_host = window.location.hostname.replace("5173", "3000");
      return `https://${codespace_host}`;
    }
  },
  created() {
    console.log('Fetching from:', `${this.baseUrl}/posts`);
    axios.get(`${this.baseUrl}/posts`)
      .then(res => { 
        console.log('API Response:', res.data);
        // Handle different response formats
        let postsData = res.data;
        // If response has a data property
        if (res.data && Array.isArray(res.data.data)) {
          postsData = res.data.data;
        }
        // If response has a posts property  
        else if (res.data && Array.isArray(res.data.posts)) {
          postsData = res.data.posts;
        }
        this.posts = Array.isArray(postsData) ? postsData : []; 
        console.log('Posts set to:', this.posts);
        // If still no posts, use mock data for testing
        if (this.posts.length === 0) {
          console.log('No posts received, using mock data');
          this.posts = [
            { id: 1, title: "Test Post", entry: "This is a test post", mood: "happy" },
            { id: 2, title: "Another Post", entry: "This is another test post", mood: "sad" }
          ];
        }
      })
      .catch(err => { 
        console.error('API Error:', err);
        console.error('API Error, using mock data for testing:', err);
        // Fallback mock data for testing
        this.posts = [
          { id: 1, title: "Test Post", entry: "This is a test post", mood: "happy" },
          { id: 2, title: "Another Post", entry: "This is another test post", mood: "sad" },
          { id: 3, entry: "Post without title", mood: "neutral" }
        ];
        this.error = "Using mock data - API unavailable: " + (err?.message || err); 
      })
      .finally(() => { 
        this.loading = false; 
        console.log('Loading finished, posts count:', this.posts.length);
      });
  }
};
</script>

<style scoped>
.ex2 { 
  max-width: 860px; 
  margin: 14px auto; 
  padding: 8px; 
}
.posts { 
  display: grid; 
  gap: 12px; 
  margin-top: 12px; 
}
.error { 
  color: #b00020; 
  background: #fff4f6; 
  padding: 8px; 
  border-radius: 6px; 
}
</style>