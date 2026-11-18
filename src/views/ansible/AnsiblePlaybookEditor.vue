<template>
  <section class="card mb-4" aria-labelledby="playbook-heading">
    <div class="card-header">
      <h2 id="playbook-heading" class="h5 mb-0">Playbook</h2>
    </div>

    <div class="card-body">
      <label for="playbook-editor">Ansible YAML</label>
      <textarea
        id="playbook-editor"
        class="form-control playbook-editor"
        spellcheck="false"
        placeholder="- hosts: all&#10;  tasks:&#10;    - name: Example task&#10;      ansible.builtin.ping:"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
      />
      <small class="form-text text-muted">
        Treat playbook content as executable code. Validate and authorize it on the backend.
      </small>
    </div>
  </section>
</template>

<script>
export default {
  name: 'AnsiblePlaybookEditor',
  props: {
    modelValue: {
      type: String,
      default: '',
    },
  },
  emits: ['update:modelValue'],
};
</script>

<style scoped>
.playbook-editor {
  min-height: 16rem;
  font-family: monospace;
  white-space: pre;
}
</style>
