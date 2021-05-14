<template>
  <div id="app">
    <section>
      <div class="head">
        <h2>Next events</h2>
        <input type="text" placeholder="Search" v-model="searchInput" />
      </div>
      <!-- CardList Container -->
      <div v-if="services.length">
        <!-- Each card Here -->
        <service-card
          v-on:update-description="handleUpdateDescription"
          v-on:handle-service-delete="handleDeleteService"
          v-for="service of filterServices()"
          v-bind:key="service.id"
          :data="service"
        />
      </div>
      <p v-else>Loading...</p>
      <p class="info-msg" v-if="!filterServices().length && searchInput">
        Oops! No events found with those keywords!
      </p>
    </section>
  </div>
</template>

<script>
//* Components
import ServiceCard from "./components/service-card.vue";

// External Packages
import axios from "axios";
const token = process.env.VUE_APP_TOKEN;

export default {
  name: "App",
  components: {
    "service-card": ServiceCard,
  },
  data() {
    return {
      services: [],
      searchInput: "",
    };
  },
  methods: {
    filterServices() {
      if (!this.searchInput) {
        return this.services;
      }

      const normalizedInput = this.searchInput.toLowerCase().trim();
      const regex = new RegExp(`${normalizedInput}`, "ig");

      const filteredArr = this.services.filter((service) => {
        return (
          service.name.toLowerCase().match(regex) ||
          service.description.toLowerCase().match(regex)
        );
      });

      return filteredArr;
    },
    handleUpdateDescription(updatedService) {
      const serviceToUpdate = this.services.find(
        (service) => service.id === updatedService.id
      );
      serviceToUpdate.description = updatedService.description;

      axios
        .post("https://challenge.pluralo.com/event/edit", serviceToUpdate, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          console.log(response);
          this.services = response.data.events;
        })
        .catch((err) => console.log(err));
    },
    handleDeleteService(serviceToDelete) {
      const filteredService = this.services.find(
        (service) => service.id === serviceToDelete.id
      );
      //! The code below makes sure the page updates according to events that got deleted
      //! In order to always have 4 events showing on the page, comment out the following lines
      // this.services = this.services.filter(
      //   (service) => service.id !== serviceToDelete.id
      // );

      axios
        .post("https://challenge.pluralo.com/event/delete", filteredService, {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => {
          //! The line below makes sure that the pages is always showing at least 4 events (according to the API response)
          this.services = response.data.events;
        })
        .catch((err) => console.log(err));
    },
  },
  created() {
    axios
      .get("https://challenge.pluralo.com/event/getall", {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      })
      .then((response) => {
        this.services = response.data.events;
      })
      .catch((err) => console.log(err));
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
  max-width: 39ch;
  font-size: 10pt;
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

.info-msg {
  background-color: #faff5e6b;
  text-align: center;
  margin: 1.5rem;
  font-size: 1.2rem;
  padding: 1rem;
  border-left: 8px solid yellow;
  box-shadow: 0px 0 2px 2px #6e6e6e1f;
}
</style>
