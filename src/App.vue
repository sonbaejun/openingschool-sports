<template>
  <div class="containerBody" row no-gutters>
    <header-component class="header" />

    <router-view class="main"></router-view>
  </div>
</template>

<script>
import HeaderComponent from "./components/HeaderComponent.vue";

export default {
  components: { HeaderComponent },
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>

<style scoped>
div {
  box-sizing: border-box;
}

html {
  margin: 0;
}

.containerBody {
  display: grid;
  height: 95vh;
  grid-template-rows: 1fr 7fr 10px 1fr;
  grid-template-areas: "header header header" " main   main   main " "   .     .      .   " "footer footer footer";
}

.header {
  grid-area: header;
}

.main {
  grid-area: main;
}

@media screen and (min-height: 1000px) {
  .containerBody {
    height: 95vh;
  }
}
</style>
