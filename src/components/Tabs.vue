<template>
  <div>
    <div class="tabs">
      <span
        v-for="({ title, active }, index) in tabs"
        :key="index"
        class="tab__header"
        :class="[`${active && 'tab__header--active'}`]"
      >
        {{ title }}
      </span>
    </div>
    <div class="tab__body">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "Tabs",
  data() {
    return {
      tabs: {},
    };
  },
  mounted() {
    this.tabs = this.$children.filter(
      ({ $options }) => $options.name === "Tab"
    );
  },
};
</script>

<style lang="scss">
.tab__header {
  &--active {
    color: red;
  }
}
</style>