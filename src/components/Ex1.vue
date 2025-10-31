<template>
  <div class="ex1">
    <label>
      Sound level
      <!-- v-model.number keeps the model as a Number (not string) -->
      <input type="number" min="0" max="3" v-model.number="soundLevel" />
    </label>

    <!-- pass the validated/clamped value to the child as a prop -->
    <SoundIcon :level="levelToShow" />
  </div>
</template>

<script>
// import the child component (make sure SoundIcon.vue is in the same folder)
import SoundIcon from "./subcomponents/SoundIcon.vue";

export default {
  name: "Ex1",
  components: { SoundIcon },

  data() {
    return {
      // bound to the input
      soundLevel: 0
    };
  },

  computed: {
    // ensure we only pass an integer 0..3 to the child
    levelToShow() {
      const n = Number(this.soundLevel);
      if (Number.isNaN(n)) return 0;
      // round and clamp to [0,3]
      return Math.max(0, Math.min(3, Math.round(n)));
    }
  }
};
</script>

<style scoped>
.ex1 {
  display: flex;
  align-items: center;
  gap: 12px;
}
input[type="number"] {
  width: 64px;
  padding: 4px;
}
</style>
