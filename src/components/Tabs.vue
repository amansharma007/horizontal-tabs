<template>
  <div class="tabs">
    <div class="tab__controls">
      <span @click="move('left')" class="tab__icon">
        <ChevronLeftIcon v-if="showLeftChevron" />
      </span>
      <div v-dragscroll class="tab__nav" ref="tab-nav">
        <span
          v-for="({ title, isTabActive }, index) in tabs"
          :key="index"
          ref="nav-item"
          class="nav__item"
          :class="[`${isTabActive ? 'active' : ''}`]"
          @click="activateTab(index)"
        >
          {{ title }}
        </span>
      </div>
      <span @click="move('right')" class="tab__icon">
        <ChevronRightIcon v-if="showRightChevron" />
      </span>
    </div>
    <slot></slot>
  </div>
</template>

<script>
import { dragscroll } from "vue-dragscroll";
import { ChevronRightIcon, ChevronLeftIcon } from "vue-feather-icons";

export default {
  name: "Tabs",
  components: {
    ChevronRightIcon,
    ChevronLeftIcon,
  },
  directives: {
    dragscroll,
  },
  props: {
    showControls: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      tabs: [],
      showLeftChevron: false,
      showRightChevron: true,
    };
  },
  created() {
    window.addEventListener("load", this.initObserver);
  },
  mounted() {
    this.tabs = this.$children.filter(
      ({ $options }) => $options.name === "Tab"
    );
  },
  computed: {
    tabNav() {
      return this.$refs["tab-nav"];
    },
    firstNavItem() {
      return this.$refs["nav-item"][0];
    },
    lastNavItem() {
      return this.$refs["nav-item"][this.$refs["nav-item"].length - 1];
    },
  },
  methods: {
    initObserver() {
      this.observer = new IntersectionObserver(this.onObserve, {
        root: this.$el,
        threshold: 1,
      });
      this.observer.observe(this.firstNavItem);
      this.observer.observe(this.lastNavItem);
    },
    onObserve(entries) {
      entries.forEach((entry) => {
        if (entry.target == this.firstNavItem) {
          this.showLeftChevron = !entry.isIntersecting;
        } else {
          this.showRightChevron = !entry.isIntersecting;
        }
      });
    },
    activateTab(tabIndex) {
      this.tabs[tabIndex].isTabActive = true;
      this.deactivateTabsExcept(tabIndex);
    },
    deactivateTabsExcept(tabIndex) {
      this.tabs.map((tab, index) => {
        tab.isTabActive = index !== tabIndex ? false : true;
      });
    },
    move(direction) {
      let { scrollLeft } = this.tabNav;
      let movement = direction === "left" ? scrollLeft - 150 : scrollLeft + 150;
      this.tabNav.scrollTo({ left: movement, behavior: "smooth" });
    },
  },
  beforeDestroy() {
    this.observer.disconnect();
    window.removeEventListener("load", this.initObserver);
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

.tab__controls {
  display: flex;
  align-items: center;
  background: #f6f6f6;
}

/* Tabs Navigation Bar */
.tab__nav {
  display: flex;
  align-items: center;
  overflow-x: scroll;
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
  margin-bottom: -2px;
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
  line-height: 1;
  color: #a4a3a4;
  user-select: none;
}
.nav__item:hover {
  background: rgb(236, 236, 236);
}
.active {
  --active-border-color: #009cfc;
  color: #444444;
  font-weight: 500;
  margin-bottom: 2px;
  border-bottom: 2px solid var(--active-border-color);
}

.tab__icon {
  display: flex;
  width: 30px;
  align-items: center;
  color: #a4a3a4;
  cursor: pointer;
}
.tab__icon:hover {
  color: #444444;
}
</style>