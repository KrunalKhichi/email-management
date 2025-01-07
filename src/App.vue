<template>
  <div id="app">
    <EmailManager />
  </div>
</template>

<script>
import EmailManager from './views/EmailManager.vue';

export default {
  name: 'App',
  components: {
    EmailManager,
  },
};
</script>

<style>
@import './assets/styles.css';
</style>
