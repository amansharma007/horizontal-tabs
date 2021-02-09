<template>
  <div id="app">
    <h1>Horizontal Tabs</h1>
    <tabs>
      <tab title="Tab 1" :active="true"> Content for 1st tab. </tab>
      <tab title="Tab 2"> Content for 2nd tab. </tab>
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
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 80vw;
  margin-left: auto;
  margin-right: auto;
}
</style>
