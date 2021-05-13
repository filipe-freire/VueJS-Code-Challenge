<template>
  <div class="service-card" :class="getAvailability">
    <div class="service-card-head">
      <div>
        <h3 class="service-title">{{ data.name }}</h3>
        <p class="date">10.03.2021 from 15.00 to 19.00 (4h)</p>
      </div>
      <div>
        <div class="availability-wrapper">
          <p>Availability</p>
          <p>{{ data.booked }}/{{ data.capacity }}</p>
        </div>
        <button-item
          v-on:handle-expand="handleExpandBtnClick"
          btn-type="regular"
          canHandleExpand
        />
      </div>
    </div>
    <div v-if="isExpanded" class="service-card-body">
      <p v-if="!isEditing">
        {{ service.description }}
      </p>
      <textarea
        v-else
        placeholder="Insira a descrição do evento aqui."
        name=""
        cols="40"
        rows="7"
        v-model="service.description"
      ></textarea>
      <div class="btn-container">
        <button-item
          v-if="!isEditing"
          @handle-edit="handleEditBtnClick"
          btn-type="edit"
        />
        <button-item
          v-if="isEditing"
          @handle-save="handleSaveBtnClick"
          btn-type="save"
        />
        <button-item
          v-if="isEditing"
          @handle-cancel="handleCancelBtnClick"
          btn-type="cancel"
        />
        <button-item btn-type="delete" />
      </div>
    </div>
  </div>
</template>

<script>
import ButtonItem from "./button-item.vue";

export default {
  data() {
    return {
      isExpanded: false,
      isEditing: false,
      service: {
        description: this.data.description,
      },
    };
  },
  components: {
    "button-item": ButtonItem,
  },
  props: {
    data: {
      type: Object,
      required: true,
    },
  },
  methods: {
    handleExpandBtnClick() {
      this.isExpanded = !this.isExpanded;
    },
    handleEditBtnClick() {
      this.isEditing = !this.isEditing;
    },
    handleSaveBtnClick() {
      this.isEditing = !this.isEditing;
      // TODO: Send request to the appropriate endpoint
    },
    handleCancelBtnClick() {
      this.isEditing = !this.isEditing;
      this.service.description = this.data.description;
    },
    handleDeleteBtnClick() {
      // TODO: Handle deleting the service
      //! call the endpoint to remove && remove from DOM (App comp)
    },
  },
  computed: {
    getAvailability() {
      const availabilityRatio = this.data.booked / this.data.capacity;
      if (availabilityRatio <= 0.2) {
        return "low";
      } else if (availabilityRatio <= 0.9) {
        return "medium";
      } else {
        return "high";
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
section {
  margin: 0 auto;
  max-width: 800px;
  padding: 1rem 2rem;
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

/* Services Card Styling */

.service-card {
  background-color: #f5f8fa;
  padding: 1rem 2rem;
  border-bottom: 1px solid #3e3e3e54;
}
.service-card.high {
  border-left: 7px solid var(--availabilityRatio-high);
}
.service-card.low {
  border-left: 7px solid var(--availabilityRatio-low);
}
.service-card.medium {
  border-left: 7px solid var(--availabilityRatio-medium);
}

.service-card-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.service-title {
  font-weight: 500;
}

.service-card-head > div:last-child {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-grow: 1;
  max-width: 194px;
}

.date {
  font-size: 10pt;
  margin-top: 0.8ch;
  color: #979797;
}

.availability-wrapper {
  text-align: right;
}

.availability-wrapper > p:nth-child(1) {
  color: #4b536b;
  font-size: 12pt;
}

.availability-wrapper > p:nth-child(2) {
  margin-top: 0.2ch;
  font-weight: 500;
}

/* SERVICE CARD BODY (description and ButtonItems) */

.service-card-body {
  margin-top: 2rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}

.service-card-body > p {
  max-width: 38ch;
  font-size: 10pt;
}

.btn-container {
  flex-grow: 1;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  align-self: flex-end;
}
</style>
