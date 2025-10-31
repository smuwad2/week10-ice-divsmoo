<script>
export default {
  name: "BlogPost2",
  props: {
    subject: { type: String, default: "Untitled" },
    entry: { type: String, default: "" },
    mood: { type: String, default: "neutral" }
  },
  computed: {
    imageSrc() {
      const moodRaw = this.mood ? String(this.mood).trim().toLowerCase() : "";
      const map = {
        happy: "/assets/happy.png",
        angry: "/assets/angry.png",
        sad: "/assets/sad.png",
        neutral: "/assets/neutral.png"
      };
      return map[moodRaw] || "/assets/neutral.png";
    },
    imageAlt() {
      return `Mood image for ${this.mood}`;
    }
  }
}
</script>

<template>
  <div class="card">
    <div class="card-body d-flex gap-3">
      <img :src="imageSrc" :alt="imageAlt" class="mood-img" />
      <div class="flex-grow-1">
        <h5 class="card-title">{{ subject }}</h5>
        <p class="card-text">{{ entry }}</p>
        <div class="text-muted small mb-2">Mood: {{ mood }}</div>
        <!-- Slot for the delete button -->
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #eee;
  border-radius: 8px;
  background: #fff;
}
.card-body {
  padding: 1rem;
}
.mood-img {
  width: 96px;
  height: 96px;
  object-fit: cover;
  border-radius: 6px;
}
.card-title {
  margin-bottom: 0.5rem;
  font-size: 1.05rem;
  font-weight: 600;
}
.card-text {
  margin-bottom: 0.5rem;
  white-space: pre-wrap;
}
</style>
