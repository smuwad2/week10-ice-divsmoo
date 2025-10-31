<template>
  <div class="task-tracker">
    <div v-if="!Array.isArray(tasks) || tasks.length === 0" class="text-muted">
      No tasks yet. Add a task above.
    </div>

    <div v-else class="task-grid">
      <!-- Use index so parent can remove exact item -->
      <div
        v-for="(t, idx) in tasks"
        :key="t.createdAt ?? idx"
        class="card mb-3 card-task"
      >
        <div class="card-body d-flex align-items-start">
          <div class="me-3">
            <!-- Inline emoji fallback (no external file) -->
            <div class="task-icon" aria-hidden="true">🗒️</div>
          </div>

          <div class="flex-grow-1">
            <h5 class="card-title mb-1 title">{{ t.title || shortTitle(t.desc) }}</h5>
            <p class="card-text entry mb-1">{{ t.desc }}</p>
            <div class="text-muted small">
              Deadline: <strong>{{ formatDeadline(t.deadline) }}</strong>
            </div>
          </div>

          <div class="ms-3 d-flex flex-column align-items-end">
            <button class="btn btn-sm btn-success mb-2" @click="$emit('done', idx)">
              Done
            </button>
            <small class="text-muted">#{{ idx + 1 }}</small>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskTracker",
  props: {
    tasks: {
      type: Array,
      required: true
    }
  },
  methods: {
  formatDeadline(d) {
    if (!d) return "No deadline";

    // If it's already an ISO YYYY-MM-DD string (from <input type="date">),
    // return it exactly so Playwright's `has-text("Deadline: 2027-12-30")` matches.
    if (/^\d{4}-\d{2}-\d{2}$/.test(d)) return d;

    // Otherwise attempt to parse and return ISO date (fallback to user string)
    try {
      const dt = new Date(d);
      if (isNaN(dt)) return d;
      // Return ISO date portion YYYY-MM-DD
      return dt.toISOString().slice(0, 10);
    } catch {
      return d;
    }
  },

  shortTitle(text) {
    if (!text) return "Task";
    return text.length > 40 ? text.slice(0, 40) + "…" : text;
  }
}

};
</script>

<style scoped>
.task-grid { margin-top: 8px; }
.card-task { box-shadow: 0 1px 2px rgba(0,0,0,0.04); }
.title { font-size: 1rem; }
.entry { white-space: pre-wrap; margin-bottom: 4px; }

/* inline emoji "icon" — no external assets required */
.task-icon {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  line-height: 1;
  border-radius: 6px;
  background: #f8f9fa;
  border: 1px solid #e9ecef;
}
</style>
