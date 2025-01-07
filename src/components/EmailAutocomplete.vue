<template>
  <div class="email-autocomplete">
    <input
      type="text"
      placeholder="Search for company domain / recipient"
      v-model="input"
      @input="filterAvailableList"
      class="inside-spacing"
    />
    <button
      v-if="isEmailValid"
      @click="addCustomEmail"
      class="cursor-pointer ml-10"
    >
      Add Email
    </button>
  </div>
</template>

<script>
import { validateEmail } from "../utils/emailUtils";

export default {
  name: "EmailAutocomplete",
  props: [],
  data() {
    return {
      input: "",
    };
  },
  computed: {
    isEmailValid() {
      return validateEmail(this.input);
    },
  },
  methods: {
    filterAvailableList() {
      this.$emit("filterAvailable", this.input);
    },
    addCustomEmail() {
      if (validateEmail(this.input)) {
        this.$emit("addRecipient", this.input);
        this.input = "";
        this.$emit("filterAvailable", "");
      } else {
        alert("Invalid email");
      }
    },
  },
};
</script>

<style scoped>
.email-autocomplete {
  margin-bottom: 1em;
}
.inside-spacing{
  padding: 5px;
}
.ml-10{
  margin-left: 10px;
  padding: 5px;
}
</style>
