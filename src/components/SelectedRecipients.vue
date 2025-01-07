<template>
  <div class="selected-recipients">
    <div
      v-for="(group, domain) in groupedSelected"
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
              :checked="true"
              @change="toggleRecipient(email, false)"
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
  name: "SelectedRecipients",
  props: {
    selected: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  computed: {
    groupedSelected() {
      return this.selected.reduce((groups, email) => {
        const domain = email.split("@")[1];
        if (!groups[domain]) groups[domain] = [];
        groups[domain].push(email);
        return groups;
      }, {});
    },
  },
  methods: {
    areAllDomainSelected(domain) {
      const emails = this.groupedSelected[domain];
      return emails.every((email) => this.selected.includes(email));
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
      const emails = this.groupedSelected[domain];
      if (isChecked) {
        emails.forEach((email) => {
          if (!this.selected.includes(email)) {
            this.$emit("selectRecipient", email);
            this.$emit("removeFromAvailable", email);
          }
        });
      } else {
        emails.forEach((email) => {
          if (this.selected.includes(email)) {
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
.selected-recipients ul {
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
