<template>
  <div id="app">
    <h1>Horizontal Tabs</h1>
    <tabs>
      <tab
        v-for="{ id, title, active } in tabsToShow"
        :key="id"
        :title="title"
        :active="active"
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
      ],
    };
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}
</style>
