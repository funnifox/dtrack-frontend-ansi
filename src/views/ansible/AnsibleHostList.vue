<template>
  <section class="card mb-4" aria-labelledby="hosts-heading">
    <div class="card-header">
      <h2 id="hosts-heading" class="h5 mb-0">Target hosts</h2>
    </div>

    <div class="card-body">
      <label for="hosts-input">Hosts</label>
      <textarea
        id="hosts-input"
        class="form-control hosts-input"
        autocomplete="off"
        spellcheck="false"
        placeholder="192.0.2.10,username,password&#10;192.0.2.11,username,password"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
      />
      <small class="form-text text-muted">
        One host per line, formatted as <code>ip,usr,pwd</code>. Don’t include commas in a field.
        Credentials are sent only when you submit; don’t save them in browser storage.
      </small>
    </div>
  </section>
</template>

<script>
export default {
  name: 'AnsibleHostList',
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
.hosts-input {
  min-height: 8rem;
  font-family: monospace;
}
</style>
