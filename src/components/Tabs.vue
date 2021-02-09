<template>
  <div class="tabs">
    <div v-dragscroll class="tab__nav">
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
import { dragscroll } from "vue-dragscroll";

export default {
  name: "Tabs",
  directives: {
    dragscroll,
  },
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
  margin-left: auto;
  margin-right: auto;
}

/* Tabs Navigation Bar */
.tab__nav {
  display: flex;
  background: #f6f6f6;
  overflow-x: scroll;
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}

/* Hide scrollbar for Chrome, Safari and Opera */
.tab__nav::-webkit-scrollbar {
  display: none;
}

/* Tabs Navigation Item */
.nav__item {
  display: block;
  padding: 10px 40px;
  cursor: pointer;
  font-size: 14px;
  color: #a4a3a4;
  user-select: none;
}
.nav__item:hover {
  background: rgb(236, 236, 236);
}
.nav__item--active {
  --active-border-color: #009cfc;
  color: #444444;
  font-weight: 500;
  border-bottom: 2px solid var(--active-border-color);
}
</style>