<template>
  <Suspense>
    <template #default>
      <Home />
    </template>
    <template #fallback>
      <Splash />
    </template>
  </Suspense>
</template>

<script>
import Home from "./components/Home.vue";
import Splash from "./components/Splash.vue";
import { defineAsyncComponent } from "vue";
export default {
  name: "App",
  components: {
    Splash,
    Home: defineAsyncComponent(() => {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve(import("./components/Home.vue"));
        }, 3000);
      });
    }),
  },
};
</script>

<style>
html,
body,
.app {
  min-height: 100vh;
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}
* {
  --brand-green: #04b500;
  --brand-blue: #0689b0;
}
</style>
