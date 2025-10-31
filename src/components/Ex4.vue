<template>
  <div class="ex4 p-3">
    <div class="mb-3">
      <label for="desc" class="form-label">Task</label>
      <input
        type="text"
        class="form-control"
        id="desc"
        v-model="desc"
        placeholder="task"
        @keyup.enter="add"
      />
    </div>

    <div class="mb-3">
      <label for="deadline" class="form-label">Deadline</label>
      <input
        type="date"
        class="form-control"
        id="deadline"
        v-model="deadline"
        placeholder="deadline"
        @keyup.enter="add"
      />
    </div>

    <button type="button" @click="add" class="btn btn-primary me-2">Add New Task</button>
    <button type="button" @click="clearAll" class="btn btn-outline-secondary">Clear All</button>

    <hr />

    <!-- Pass the tasks to TaskTracker and listen for 'done' -->
    <TaskTracker
      :tasks="taskList"
      @done="deleteTask"
      class="mt-3"
    />
  </div>
</template>

<script>
import TaskTracker from "./subcomponents/TaskTracker.vue";

export default {
  name: "Ex4",
  components: { TaskTracker },

  data() {
    return {
      desc: "",
      deadline: "",
      taskList: []
    };
  },

  methods: {
    // Add a new task (ignore empty descriptions)
    add() {
      const trimmed = (this.desc || "").trim();
      if (!trimmed) return; // don't add blank tasks

      // store as an object so we can expand later (e.g., completed flag)
      this.taskList.push({
        desc: trimmed,
        deadline: this.deadline || null, // keep null if empty
        createdAt: new Date().toISOString()
      });

      // reset inputs
      this.desc = "";
      this.deadline = "";
    },

    // Remove a task when child emits 'done' with the index
    deleteTask(index) {
      // defensive check
      if (typeof index !== "number") return;
      if (index < 0 || index >= this.taskList.length) return;
      this.taskList.splice(index, 1);
    },

    // Optional helper to clear all tasks
    clearAll() {
      this.taskList = [];
    }
  }
};
</script>

<style scoped>
.ex4 { max-width: 700px; margin: 0 auto; }
</style>
