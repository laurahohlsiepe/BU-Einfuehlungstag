<template>
  <HelloWorld msg="Welcome to Your Vue.js App" />
</template>

<script>
import "@/assets/style.css";
import axios from "axios";

import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  data() {
    return {
      items: [],
    };
  },

  components: {
    HelloWorld,
  },

  async created() {
    try {
      const res = await axios.get(`http://localhost:3000/items`);
      this.items = res.data;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<style></style>
