<template>
  <div id="app">
    <section>
      <div class="head">
        <h2>Next events</h2>
        <input type="text" placeholder="Search" />
      </div>
      <!-- CardList Container -->
      <div v-if="services.length">
        <!-- Each card Here -->

        <service-card
          v-for="service of services"
          v-bind:key="service.id"
          :data="service"
        />
      </div>
      <p v-else>Loading...</p>
    </section>
  </div>
</template>

<script>
import ServiceCard from "./components/service-card.vue";
import mockServices from "./assets/getall.json";

export default {
  name: "App",
  components: {
    "service-card": ServiceCard,
  },
  data() {
    return {
      services: null,
    };
  },
  created() {
    this.services = mockServices.events;
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap");
/* Custom Properties */
:root {
  /* COLORS */
  --availabilityRatio-low: #6ac951;
  --availabilityRatio-medium: #ffc764;
  --availabilityRatio-high: #ff5533;

  /* BUTTONS */
  --bg-save: #6ac951;
  --bg-save-border: rgba(74, 82, 106, 0.35);

  --bg-regular: #f1f1f8;
  --bg-regular-border: #d3d7db;

  --bg-delete: #f16c64;
  --bg-delete-border: rgba(241, 108, 100, 0.35);

  --bg-edit: #00a9f0;
  --bg-edit-border: rgba(0, 169, 240, 0.4);

  /* FONTS */
  --font-family-primary: "Roboto", sans-serif;
}

/* Styling reset */
*,
::before,
::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: var(--font-family-primary);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

section {
  margin: 0 auto;
  max-width: 800px;
  padding: 1rem 2rem;
}

textarea {
  font-family: inherit;
}

.head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1em;
}

.head > input {
  flex-grow: 1;
  max-width: 200px;
  padding: 1ch;
}
</style>
