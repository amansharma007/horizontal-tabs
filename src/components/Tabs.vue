<template>
  <div class="tabs">
    <div class="tab__row">
      <span @click="move('left')" class="controls__icon">
        <ChevronLeftIcon v-if="showLeftChevron" />
      </span>
      <div v-dragscroll class="tab__nav" ref="tab-nav">
        <span
          v-for="({ title, isTabActive }, index) in tabs"
          :key="index"
          ref="nav-item"
          class="nav__item"
          :class="[`${isTabActive ? 'nav__item--active' : ''}`]"
          @click="activateTab(index)"
        >
          <span
            @click.stop="onTabCloseInit(title, index)"
            class="nav__item__close"
            v-if="closable"
          >
            <XIcon />
          </span>
          {{ title }}
        </span>
      </div>
      <span @click="move('right')" class="controls__icon">
        <ChevronRightIcon v-if="showRightChevron" />
      </span>
    </div>

    <slot></slot>

    <modal v-show="showModal">
      <h3 slot="header">Close {{ tabToClose.title }}</h3>
      <p slot="body">Are you sure you want to close {{ tabToClose.title }}?</p>
      <template slot="footer">
        <button class="button button--primary" @click="showModal = false">
          No
        </button>
        <button class="button button--danger" @click="onTabCloseConfirm()">
          Yes
        </button>
      </template>
    </modal>
  </div>
</template>

<script>
import Modal from "@/components/Modal.vue";
import { dragscroll } from "vue-dragscroll";
import { ChevronRightIcon, ChevronLeftIcon, XIcon } from "vue-feather-icons";

export default {
  name: "Tabs",
  components: {
    ChevronRightIcon,
    ChevronLeftIcon,
    XIcon,
    Modal,
  },
  directives: {
    dragscroll,
  },
  props: {
    showControls: {
      type: Boolean,
      default: false,
    },
    closable: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      tabs: [],
      showLeftChevron: false,
      showRightChevron: true,
      showModal: false,
      tabToClose: {},
    };
  },
  created() {
    window.addEventListener("load", this.initObserver);
  },
  mounted() {
    this.initTabs();
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
    initTabs() {
      this.tabs = this.$children.filter(
        ({ $options }) => $options.name === "Tab"
      );
    },
    initObserver() {
      this.observer = new IntersectionObserver(this.onObserve, {
        root: this.$el,
        threshold: 1,
      });
      this.observer.observe(this.firstNavItem);
      this.observer.observe(this.lastNavItem);
    },
    onTabCloseInit(title, index) {
      this.showModal = true;
      this.tabToClose = {
        title,
        index,
      };
    },
    onTabCloseConfirm() {
      this.$emit("close-tab", this.tabToClose.index);
      this.showModal = false;
      this.$nextTick().then(() => {
        this.initTabs();
      });
    },
    onTabAdded() {
      this.$nextTick().then(() => {
        this.initTabs();
      });
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
@import "../assets/styles/tabs.css";
</style>
