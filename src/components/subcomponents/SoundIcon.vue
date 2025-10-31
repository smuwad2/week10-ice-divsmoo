<template>
  <!-- single span element as requested.
       data-testid makes Playwright selectors reliable,
       role/aria-label improve accessibility. -->
  <span
    data-testid="sound-icon"
    role="img"
    :aria-label="`Sound level ${idx}`"
    :title="`Sound level ${idx}`"
  >
    {{ displayEmoji }}
  </span>
</template>

<script>
export default {
  name: "SoundIcon",

  // the parent passes a numeric level (0..3)
  props: {
    level: {
      type: Number,
      required: true,
      validator(v) {
        // accept only integers 0..3 (helps catch mistakes early)
        return Number.isInteger(v) && v >= 0 && v <= 3;
      }
    }
  },

  data() {
    return {
      // keep your original emojis here
      soundEmojis: ["🔇", "🔈", "🔉", "🔊"]
    };
  },

  computed: {
    // clamp/round index to 0..3 so we never access out-of-range
    idx() {
      const n = Number(this.level);
      if (Number.isNaN(n)) return 0;
      return Math.max(0, Math.min(3, Math.round(n)));
    },
    displayEmoji() {
      return this.soundEmojis[this.idx] ?? this.soundEmojis[0];
    }
  }
};
</script>

<style scoped>
/* optional: make emoji a bit larger for visibility */
span[data-testid="sound-icon"] {
  font-size: 28px;
  line-height: 1;
}
</style>
