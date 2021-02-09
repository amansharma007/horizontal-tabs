<template>
  <div v-if="isTabActive" class="tab__body">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "Tab",
  props: {
    title: {
      type: String,
      required: true,
    },
    active: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isTabActive: false
    };
  },
  mounted() {
    this.isTabActive = this.active
  }
};
</script>

<style lang="css">
/* Visible Tab Body */
.tab__body {
  margin: 10px;
  border: 1px dotted lightgray;
  padding: 10px;
  font-size: 30px;
}
</style>