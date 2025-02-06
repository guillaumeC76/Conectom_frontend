
<template>
  <div>
    <h1>Dashboard Conectom</h1>
    <p v-if="message">{{ message }}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'HomeView',
  data() {
    return {
      message: "",
    };
  },
  async created() {
    try {
      const response = await axios.get("http://localhost:3000/");
      this.message = response.data;
    } catch (error) {
      console.error("Erreur API", error);
    }
  },
};
</script>
