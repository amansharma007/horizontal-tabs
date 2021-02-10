<template>
  <div v-if="isTabActive">
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
      default: false,
    },
  },
  data() {
    return {
      isTabActive: false,
    };
  },
  mounted() {
    this.isTabActive = this.active;
  },
};
</script>
