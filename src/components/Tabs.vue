<template>
  <div class="tabs">
    <div class="tab__nav">
      <span
        v-for="({ title, isTabActive }, index) in tabs"
        :key="index"
        class="nav__item"
        :class="[`${isTabActive && 'nav__item--active'}`]"
        @click="activateTab(index)"
      >
        {{ title }}
      </span>
    </div>
    <slot></slot>
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
  methods: {
    activateTab(tabIndex) {
      this.tabs[tabIndex].isTabActive = true;
      this.deactivateTabsExcept(tabIndex);
    },
    deactivateTabsExcept(tabIndex) {
      this.tabs.map((tab, index) => {
        tab.isTabActive = index !== tabIndex ? false : true;
      });
    },
  },
};
</script>

<style lang="css">
/* TODO: 
- Add the colors to variables
- Segregate all the css into files which will be stored in assets folder.  
*/
/* Tabs */
.tabs {
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

/* Tabs Navigation Bar */
.tab__nav {
  --border_color: black;
  display: flex;
  border-bottom: 1px solid var(--border_color);
}

/* Tabs Navigation Item */
.nav__item {
  display: block;
  padding: 10px 20px;
  cursor: pointer;
  font-size: 14px;
}
.nav__item:hover {
  background: lightgrey;
}
.nav__item--active {
  --active-border-color: blue;
  margin-bottom: -1px;
  border-bottom: 2px solid var(--active-border-color);
}
</style>