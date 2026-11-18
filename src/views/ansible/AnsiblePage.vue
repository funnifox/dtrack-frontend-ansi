<template>
  <main class="ansible-page">
    <h1>Ansible</h1>
    <p class="text-muted">
      Configure target hosts and a playbook, then submit a run to your authorized runner.
    </p>

    <form @submit.prevent="submitRun">
      <div class="ansible-workspace">
        <AnsibleHostList v-model="hosts" />
        <AnsiblePlaybookEditor v-model="playbook" />
      </div>

      <div class="run-actions">
        <button
          class="btn btn-primary"
          type="submit"
          :disabled="!canRun || running"
        >
          {{ running ? 'Submitting…' : 'Run playbook' }}
        </button>

        <span v-if="status" class="text-muted" role="status">
          {{ status }}
        </span>
      </div>
    </form>

    <AnsibleRunOutput :output="output" :status="runStatus" />
  </main>
</template>

<script>
import AnsibleHostList from './AnsibleHostList.vue';
import AnsiblePlaybookEditor from './AnsiblePlaybookEditor.vue';
import AnsibleRunOutput from './AnsibleRunOutput.vue';

export default {
  name: 'AnsiblePage',
  components: {
    AnsibleHostList,
    AnsiblePlaybookEditor,
    AnsibleRunOutput,
  },
  emits: ['submit-run'],
  data() {
    return {
      hosts: '',
      playbook: '',
      output: '',
      runStatus: 'Idle',
      running: false,
      status: '',
    };
  },
  computed: {
    parsedHosts() {
      return this.hosts
        .split('\n')
        .map((line) => line.trim())
        .filter(Boolean)
        .map((line) => {
          const [address, username, password, ...extra] = line.split(',');
          const trimmedAddress = address?.trim() || '';
          const trimmedUsername = username?.trim() || '';

          return {
            address: trimmedAddress,
            username: trimmedUsername,
            password: password || '',
            valid:
              extra.length === 0 &&
              Boolean(trimmedAddress && trimmedUsername && password),
          };
        });
    },
    canRun() {
      return (
        this.parsedHosts.length > 0 &&
        this.parsedHosts.every((host) => host.valid) &&
        this.playbook.trim().length > 0
      );
    },
  },
  methods: {
    submitRun() {
      if (!this.canRun || this.running) return;

      this.status = 'Run request submitted.';
      this.$emit('submit-run', {
        hosts: this.parsedHosts.map(({ address, username, password }) => ({
          address,
          username,
          password,
        })),
        playbook: this.playbook,
      });
    },
  },
};
</script>

<style scoped>
.ansible-page {
  padding: 1.25rem;
}

.ansible-workspace {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(0, 4fr);
  gap: 1rem;
  align-items: stretch;
}

.ansible-workspace > * {
  height: 100%;
}

.run-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin: 1rem 0 1.5rem;
}

@media (max-width: 768px) {
  .ansible-workspace {
    grid-template-columns: 1fr;
  }
}
</style>
