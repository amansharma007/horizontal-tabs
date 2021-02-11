<template>
  <div class="tabs">
    <div class="tab__row">
      <div class="controls__icon" v-if="$slots['prefix']">
        <slot name="prefix"></slot>
      </div>

      <div class="controls__icon" @click="scroll('left')">
        <ChevronLeftIcon v-if="showLeftChevron" />
      </div>

      <div class="tab__nav" v-dragscroll ref="tab-nav">
        <div
          class="nav__item"
          :class="[`${isTabActive ? 'nav__item--active' : ''}`]"
          v-for="({ title, isTabActive }, index) in tabs"
          :key="index"
          ref="nav-item"
          @click="activateTab(index)"
        >
          <div
            class="nav__item__close"
            v-if="closable && tabs.length > 1"
            @click.stop="openTabCloseModal(title, index)"
          >
            <XIcon />
          </div>
          {{ title }}
        </div>
      </div>

      <div class="controls__icon" @click="scroll('right')">
        <ChevronRightIcon v-if="showRightChevron" />
      </div>

      <div class="controls__icon" v-if="$slots['suffix']">
        <slot name="suffix"></slot>
      </div>
    </div>

    <slot></slot>

    <modal v-show="showModal">
      <h3 slot="header">Close {{ tabToClose.title }}</h3>
      <p slot="body">Are you sure you want to close {{ tabToClose.title }}?</p>
      <template slot="footer">
        <button class="button button--primary" @click="closeTabCloseModal()">
          No
        </button>
        <button class="button button--danger" @click="closeTab()">
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
    Modal,
    XIcon,
    ChevronLeftIcon,
    ChevronRightIcon,
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
      tabToClose: {},
      showModal: false,
      showLeftChevron: false,
      showRightChevron: false,
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
      /**
       * `this.tabs` is assigned the list of `<tab>` components,
       * which are also the children of this component, via slots.
       */
      this.tabs = this.$children.filter(
        ({ $options }) => $options.name === "Tab"
      );
    },
    initObserver() {
      this.observer = new IntersectionObserver(this.onNavScrollObserve, {
        root: this.$el,
        threshold: 1,
      });
      this.observer.observe(this.firstNavItem);
      this.observer.observe(this.lastNavItem);
    },
    onNavScrollObserve(entries) {
      entries.forEach((entry) => {
        if (entry.target == this.lastNavItem) {
          this.showRightChevron = !entry.isIntersecting;
        } else {
          this.showLeftChevron = !entry.isIntersecting;
        }
      });
    },
    onTabAdded() {
      /**
       * This function is to be used in `<tabs>` component's parent,
       * to refresh the tabs array here (which is `this.$children`).
       * This is done because `this.$children` is not reactive by default.
       */
      this.$nextTick()
        .then(() => {
          this.initTabs();
        })
        .then(() => {
          this.scrollToLastNavItem();
          this.observer.observe(
            document.querySelector(".nav__item:first-child")
          );
          this.observer.observe(
            document.querySelector(".nav__item:last-child")
          );
        });
    },
    scrollToLastNavItem() {
      this.tabNav.scrollTo({
        left: document.querySelector(".nav__item:last-child").offsetLeft,
        behavior: "smooth",
      });
    },
    scroll(direction) {
      let { scrollLeft } = this.tabNav;
      let movement = direction === "left" ? scrollLeft - 150 : scrollLeft + 150;
      this.tabNav.scrollTo({ left: movement, behavior: "smooth" });
    },
    openTabCloseModal(title, index) {
      this.showModal = true;
      this.tabToClose = { title, index };
    },
    closeTabCloseModal() {
      this.showModal = false;
    },
    closeTab() {
      this.$emit("close-tab", this.tabToClose.index);
      this.closeTabCloseModal();
      this.$nextTick().then(() => {
        this.initTabs();
        this.activateTab(0);
      });
    },
    activateTab(tabIndex) {
      this.tabs[tabIndex].isTabActive = true;
      this.deactivateAllTabsExcept(tabIndex);
    },
    deactivateAllTabsExcept(tabIndex) {
      this.tabs.map((tab, index) => {
        tab.isTabActive = index !== tabIndex ? false : true;
      });
    },
  },
  beforeDestroy() {
    window.removeEventListener("load", this.initObserver);
  },
};
</script>

<style lang="css">
@import "../assets/styles/tabs.css";
</style>
