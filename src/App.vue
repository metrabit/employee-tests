<template>
  <div id="app">
    <TestsPage />
  </div>
</template>

<script>
import TestsPage from "./page/TestsPage";

export default {
  name: "App",
  components: {
    TestsPage
  }
};
</script>

<style lang="scss">
@import "./page/style/style.scss";
</style>
