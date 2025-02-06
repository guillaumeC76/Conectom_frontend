<template>
  <div>
    <h1>Dashboard IoT</h1>
    <div v-if="lights.length">
      <div v-for="(light, id) in lights" :key="id" class="light">
        <h3>{{ light.name }}</h3>
        <button @click="toggleLight(id, !light.state.on)">
          {{ light.state.on ? "Éteindre" : "Allumer" }}
        </button>
      </div>
    </div>
    <p v-else>Chargement des lumières...</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      lights: [],
    };
  },
  async created() {
    await this.fetchLights();
  },
  methods: {
    async fetchLights() {
      try {
        const response = await axios.get("http://localhost:3000/hue/lights");
        this.lights = response.data;
      } catch (error) {
        console.error("Erreur API", error);
      }
    },
    async toggleLight(id, state) {
      try {
        await axios.post(`http://localhost:3000/hue/lights/${id}`, { on: state });
        this.lights[id].state.on = state;
      } catch (error) {
        console.error("Impossible de changer la lumière", error);
      }
    },
  },
};
</script>

<style>
.light {
  margin: 10px;
  padding: 10px;
  border: 1px solid #ccc;
}
</style>
