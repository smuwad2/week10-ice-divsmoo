<template>
  <!-- wrapper uses class "card" so existing tests that look for div.card work -->
  <div class="card blog-post" data-testid="blog-post">
    <img :src="imageSrc" :alt="imageAlt" class="mood-img" />
    <div class="body">
      <!-- Title: test expects card to contain a title like "My First Love" -->
      <h3 class="title">{{ displayTitle }}</h3>
      <!-- main entry stays a paragraph -->
      <p class="entry">{{ post.entry }}</p>
      <!-- meta kept as div so locator('p') remains unambiguous -->
      <div class="meta">Mood: {{ post.mood ?? "Unknown" }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "BlogPost",
  props: {
    post: { type: Object, required: true }
  },
  computed: {
    // Title: prefer explicit post.subject (from DB), fallback to title, entry or 'Untitled'
    displayTitle() {
      if (!this.post) return "Untitled";
      return this.post.subject ?? this.post.title ?? this.post.entry ?? "Untitled";
    },
    imageSrc() {
      const moodRaw = (this.post && this.post.mood) ? String(this.post.mood).trim().toLowerCase() : "";
      const map = {
        happy: "/assets/happy.png",
        angry: "/assets/angry.png",
        sad: "/assets/sad.png",
        neutral: "/assets/neutral.png"
      };
      return map[moodRaw] || "/assets/neutral.png";
    },
    imageAlt() {
      return `Mood image for ${this.post.mood ?? "post"}`;
    }
  }
};
</script>

<style scoped>
.card { 
  display: flex; 
  gap: 12px; 
  padding: 10px; 
  border: 1px solid #eee; 
  border-radius: 8px; 
  background: #fff; 
}
.blog-post { 
  align-items: flex-start; 
}
.mood-img { 
  width: 96px; 
  height: 96px; 
  object-fit: cover; 
  border-radius: 6px; 
}
.body { 
  flex: 1; 
}
.title { 
  margin: 0 0 6px; 
  font-size: 1.05rem; 
  font-weight: 600; 
}
.entry { 
  margin: 0 0 8px; 
  white-space: pre-wrap; 
}
.meta { 
  margin: 0; 
  color: #666; 
  font-size: 0.9rem; 
}
</style>