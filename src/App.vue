<template>
  <div id="app">
    <h1>Draggable Horizontal Tabs</h1>
    <tabs :show-controls="true" :closable="true" @close-tab="closeTab">
      <tab
        v-for="{ id, title, active } in tabsToShow"
        :key="id"
        :title="title"
        :active="active"
        class="tab__body"
      >
        Tab {{ id }} contents
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
      tabsToShow: [
        {
          id: 1,
          title: "Tab1",
          active: true,
        },
        {
          id: 2,
          title: "Tab2",
          active: false,
        },
        {
          id: 3,
          title: "Tab3",
          active: false,
        },
        {
          id: 4,
          title: "Tab4",
          active: false,
        },
        {
          id: 5,
          title: "Tab5",
          active: false,
        },
        {
          id: 6,
          title: "Tab6",
          active: false,
        },
        {
          id: 7,
          title: "Tab7",
          active: false,
        },
        {
          id: 8,
          title: "Tab8",
          active: false,
        },
        {
          id: 9,
          title: "Tab9",
          active: false,
        },
        {
          id: 10,
          title: "Tab10",
          active: false,
        },
      ],
    };
  },
  methods: {
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
</style>
