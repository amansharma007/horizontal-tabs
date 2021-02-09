<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "Tab",
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      active: true,
    };
  },
};
</script>

<style>
</style>