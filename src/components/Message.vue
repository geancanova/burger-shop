<template>
  <div class="message-container" v-show="visible">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: 'Message',
  props: {
    msg: String
  },
  data() {
    return {
      visible: false
    }
  },
  watch: {
    msg: function() {
      this.visible = this.msg !== '' ? true : false;
      if (this.visible) setTimeout(() => this.visible = false, 3000);
    }
  }
}
</script>

<style scoped>
  .message-container {
    max-width: 400px;
    margin: 30px auto;
    background-color: var(--color-secondary);
    border-radius: 10px;
    color: var(--color-primary);
    padding: 1em 2em;
  }
</style>