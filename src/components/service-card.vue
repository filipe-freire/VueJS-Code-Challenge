<template>
  <div class="service-card" :class="getAvailability">
    <div class="service-card-head">
      <div>
        <h3 class="service-title">{{ data.name }}</h3>
        <p class="date">{{ service.formattedDate }}</p>
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
    <transition name="fade">
      <div v-if="isExpanded" class="service-card-body">
        <p v-if="!isEditing">
          {{ data.description }}
        </p>
        <textarea
          v-else
          placeholder="Insira a descrição do evento aqui."
          cols="40"
          rows="7"
          v-model="service.description"
        ></textarea>
        <div class="btn-container">
          <button-item
            v-if="!isEditing"
            @handle-edit="toggleEdit"
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
          <button-item
            @handle-delete="handleDeleteBtnClick"
            btn-type="delete"
          />
        </div>
      </div>
    </transition>
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
        formattedDate: "",
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
    toggleEdit() {
      this.isEditing = !this.isEditing;
    },
    handleExpandBtnClick() {
      this.isExpanded = !this.isExpanded;
    },
    handleSaveBtnClick() {
      // runs if the user clicks save even if no alteration has been made
      if (this.service.description !== this.data.description) {
        this.$emit("update-description", {
          description: this.service.description,
          id: this.data.id,
        });
      }
      this.toggleEdit();
    },
    handleCancelBtnClick() {
      this.service.description = this.data.description;
      this.toggleEdit();
    },
    handleDeleteBtnClick() {
      this.$emit("handle-service-delete", { id: this.data.id });
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
  created() {
    const startDate = new Date(this.data.eventStart);
    const endingDate = new Date(this.data.eventEnd);

    const day = startDate.getDate();
    const month = startDate.getMonth() + 1;
    const year = startDate.getFullYear();

    const hoursStart = startDate.getHours();
    const minutesStart =
      startDate.getMinutes() < 10 ? "00" : startDate.getMinutes();

    const hoursEnd = endingDate.getHours();
    const minutesEnd =
      endingDate.getMinutes() < 10 ? "00" : endingDate.getMinutes();

    const duration = hoursEnd - hoursStart;

    this.service.formattedDate = `${day}/${month}/${year} from ${hoursStart}.${minutesStart} to ${hoursEnd}.${minutesEnd} (${duration}h)`;
  },
};
</script>

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
.fade-enter-active {
  transition: all 0.4s ease;
}
.fade-enter {
  opacity: 0;
  transform: translateY(-30px);
}

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
