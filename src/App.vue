<template>
  <div id="app">
    <h1>Draggable Horizontal Tabs</h1>
    <button id="add-tab-btn" class="button button--primary" @click="addTab()">
      Add Tab
    </button>
    <tabs
      ref="tabs"
      :show-controls="true"
      :closable="true"
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
    </tabs>
  </div>
</template>

<script>
import Tabs from "./components/Tabs.vue";
import Tab from "./components/Tab.vue";

export default {
  name: "App",
  components: {
    Tabs,
    Tab,
  },
  data() {
    return {
      tabsToShow: [],
    };
  },
  created() {
    // Generate a list of tabs.
    for (let i = 1; i <= 8; i++) {
      this.tabsToShow.push(this.getNewTab(i, `Tab ${i}`, false));
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
      this.tabsToShow.push(
        this.getNewTab(this.tabsToShow.length + 1, "New Tab", false)
      );
      this.$refs["tabs"].onTabAdded();
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

#add-tab-btn {
  margin-bottom: 10px;
}
</style>
