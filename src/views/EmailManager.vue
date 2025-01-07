<template>
  <div class="email-manager">
    <div class="email-section">
      <div class="email-section-inner">
        <h2 class="recipients-heading">Available Recipients</h2>
        <EmailAutocomplete
          :recipients="recipients"
          @filterAvailable="handleFilterAvailable"
          @addRecipient="handleAddToAvailable"
        />
        <AvailableRecipients
          :recipients="filteredRecipients"
          :selected="selectedRecipients"
          @selectRecipient="handleSelectRecipient"
          @removeRecipient="handleRemoveRecipient"
        />
      </div>
      <div class="email-section-inner">
        <h2 class="recipients-heading">Selected Recipients</h2>
        <SelectedRecipients
          :selected="selectedRecipients"
          @removeRecipient="handleRemoveRecipient"
        />
      </div>
    </div>
  </div>
</template>

<script>
import AvailableRecipients from "../components/AvailableRecipients.vue";
import SelectedRecipients from "../components/SelectedRecipients.vue";
import EmailAutocomplete from "../components/EmailAutocomplete.vue";
import { recipients as initialRecipients } from "../data/recipientsData.js";

export default {
  name: "EmailManager",
  components: {
    AvailableRecipients,
    SelectedRecipients,
    EmailAutocomplete,
  },
  data() {
    return {
      recipients: [...initialRecipients], 
      filterText: "",
    };
  },
  computed: {
    filteredRecipients() {
      const filter = this.filterText.toLowerCase();
      return this.recipients.filter(
        (recipient) =>
          recipient.email.toLowerCase().includes(filter) && !recipient.isSelected
      );
    },
    selectedRecipients() {
      return this.recipients
        .filter((recipient) => recipient.isSelected)
        .map((recipient) => recipient.email);
    },
  },
  methods: {
    handleSelectRecipient(email) {
      const recipient = this.recipients.find((r) => r.email === email);
      if (recipient) {
        recipient.isSelected = true;
      }
    },
    handleRemoveRecipient(email) {
      const recipient = this.recipients.find((r) => r.email === email);
      if (recipient) {
        recipient.isSelected = false;
      }
    },
    handleAddToAvailable(email) {
      if (!this.recipients.some((e) => e.email === email)) {
        this.recipients.push({ email, isSelected: false });
      }
    },
    handleRemoveFromAvailable(email) {
      this.recipients = this.recipients.filter((e) => e.email !== email);
    },
    handleFilterAvailable(input) {
      this.filterText = input;
    },
  },
};
</script>

<style scoped>
.email-manager {
  padding: 20px;
}
.email-section {
  display: flex;
  gap: 20px;
}
.email-section-inner {
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 50%;
}
.recipients-heading {
  font-weight: 600;
}
</style>
