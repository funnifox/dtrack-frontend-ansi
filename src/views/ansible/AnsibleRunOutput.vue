<template>
  <section class="card" aria-labelledby="output-heading">
    <div class="card-header output-header">
      <h2 id="output-heading" class="h5 mb-0">Run output</h2>
      <span class="badge badge-secondary">{{ status }}</span>
    </div>

    <div
      class="card-body output-panel"
      role="log"
      aria-live="polite"
      aria-atomic="false"
    >
      <pre>{{ output || 'Run output will appear here.' }}</pre>
    </div>
  </section>
</template>

<script>
export default {
  name: 'AnsibleRunOutput',
  props: {
    output: {
      type: String,
      default: '',
    },
    status: {
      type: String,
      default: 'Idle',
    },
  },
};
</script>

<style scoped>
.output-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.output-panel {
  min-height: 10rem;
  max-height: 28rem;
  overflow: auto;
  background: #17191c;
  color: #e7e7e7;
}

pre {
  margin: 0;
  white-space: pre-wrap;
  overflow-wrap: anywhere;
  font-family: monospace;
}
</style>
