<!-- src/components/Ex2.vue -->
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
  data() { return { posts: [], loading: true, error: "" }; },
  computed: {
    baseUrl() {
      if (window.location.hostname === "localhost") return "http://localhost:3000";
      const codespace_host = window.location.hostname.replace("5173", "3000");
      return `https://${codespace_host}`;
    }
  },
  created() {
    axios.get(`${this.baseUrl}/posts`)
      .then(res => { this.posts = Array.isArray(res.data) ? res.data : []; })
      .catch(err => { this.error = "Failed to load posts: " + (err?.message || err); })
      .finally(() => { this.loading = false; });
  }
};
</script>

<style scoped>
.ex2 { max-width:860px; margin:14px auto; padding:8px; }
.posts { display:grid; gap:12px; margin-top:12px; }
.error { color:#b00020; background:#fff4f6; padding:8px; border-radius:6px; }
</style>
