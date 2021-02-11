<template>
  <div id="app">
    <h1>Draggable Horizontal Tabs</h1>
    <tabs
      ref="tabs"
      :closable="true"
      :show-controls="true"
      @close-tab="closeTab"
    >
      <tab
        v-for="{ id, title, active } in tabsToShow"
        :key="id"
        :title="title"
        :active="active"
        class="tab__body"
      >
        "{{ title }}" Content
      </tab>
      <template slot="suffix">
        <div id="add-tab-icon" @click="addTab()">
          <PlusIcon />
        </div>
      </template>
    </tabs>

    <modal v-show="showModal">
      <h3 slot="header">Can't add more.</h3>
      <p slot="body">
        You are not allowed to add more than 10 tabs. If you want to add more
        tabs then close a few and continue.
      </p>
      <template slot="footer">
        <button class="button button--primary" @click="showModal = false">
          Ok
        </button>
      </template>
    </modal>
  </div>
</template>

<script>
import Tabs from "./components/Tabs.vue";
import Tab from "./components/Tab.vue";
import Modal from "./components/Modal.vue";
import { PlusIcon } from "vue-feather-icons";
import { v4 as uuid } from "uuid";

const MAX_TABS = 10;
const INITIAL_TABS = 3;

export default {
  name: "App",
  components: {
    Tabs,
    Tab,
    Modal,
    PlusIcon,
  },
  data() {
    return {
      tabsToShow: [],
      showModal: false,
    };
  },
  created() {
    // Generate a list of tabs.
    for (let i = 1; i <= INITIAL_TABS; i++) {
      this.tabsToShow.push(this.getNewTab(uuid(), `Tab ${i}`, false));
    }
    // Set first tab as active by default.
    this.tabsToShow[0].active = true;
  },
  methods: {
    getNewTab(id, title, active) {
      return {
        id,
        title,
        active,
      };
    },
    addTab() {
      if (this.tabsToShow.length < MAX_TABS) {
        this.tabsToShow.push(this.getNewTab(uuid(), "New Tab", false));
        this.$refs["tabs"].onTabAdded();
      } else {
        this.showModal = true;
      }
    },
    closeTab(tabIndex) {
      this.tabsToShow = this.tabsToShow.filter(
        (elem, index) => index !== tabIndex
      );
    },
  },
};
</script>

<style lang="css">
@import "./assets/styles/app.css";

#add-tab-icon {
  display: flex;
}
</style>
