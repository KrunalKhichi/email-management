<template>
  <div class="available-recipients">
    <div
      v-for="(group, domain) in groupedRecipients"
      :key="domain"
    >
      <h3>
        <label class="cursor-pointer">
          {{ domain }}
          <input
            type="checkbox"
            :checked="areAllDomainSelected(domain)"
            @change="toggleDomain(domain, $event.target.checked)"
          />
        </label>
      </h3>
      <ul>
        <li
          v-for="email in group"
          :key="email"
          class="email-list"
        >
          <label class="cursor-pointer">
            <input
              type="checkbox"
              :checked="isSelected(email)"
              @change="toggleRecipient(email, $event.target.checked)"
            />
            {{ email }}
          </label>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "AvailableRecipients",
  props: {
    recipients: {
      type: Array,
      required: true,
    },
    selected: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  computed: {
    groupedRecipients() {
      return this.recipients.reduce((groups, recipient) => {
        const domain = recipient.email.split("@")[1];
        if (!groups[domain]) groups[domain] = [];
        groups[domain].push(recipient.email);
        return groups;
      }, {});
    },
  },
  methods: {
    isSelected(email) {
      return this.selected.includes(email);
    },
    areAllDomainSelected(domain) {
      const emails = this.groupedRecipients[domain];
      return emails.every((email) => this.isSelected(email));
    },
    toggleRecipient(email, isChecked) {
      if (isChecked) {
        this.$emit("selectRecipient", email);
        this.$emit("removeFromAvailable", email);
      } else {
        this.$emit("removeRecipient", email);
        this.$emit("addToAvailable", email);
      }
    },
    toggleDomain(domain, isChecked) {
      const emails = this.groupedRecipients[domain];
      if (isChecked) {
        emails.forEach((email) => {
          if (!this.isSelected(email)) {
            this.$emit("selectRecipient", email);
            this.$emit("removeFromAvailable", email);
          }
        });
      } else {
        emails.forEach((email) => {
          if (this.isSelected(email)) {
            this.$emit("removeRecipient", email);
            this.$emit("addToAvailable", email);
          }
        });
      }
    },
  },
};
</script>

<style scoped>
.available-recipients ul {
  list-style: none;
  padding: 0;
}

h3 {
  display: flex;
  align-items: center;
}

h3 label {
  margin-right: 10px;
}
</style>
