<template>
  <div id="app">
    <div class="progress-bar">
      <div :class="{ active: step >= 1 }" @click="goToStep(1)">1</div>
      <div :class="{ active: step >= 2 }" @click="goToStep(2)">2</div>
      <div :class="{ active: step >= 3 }" @click="goToStep(3)">3</div>
      <div :class="{ active: step >= 4 }" @click="goToStep(4)">4</div>
    </div>
    <component
      :is="currentComponent"
      @next-step="nextStep"
      @go-back="goBack"
      @update-personal-info="updatePersonalInfo"
      :personalInfo="personalInfo"
      :event="selectedEvent"
      :session="selectedSession"
    />
  </div>
</template>

<script>
import { ref, computed } from "vue";
import SignupForm from "./components/SignupForm.vue";
import EventList from "./components/EventList.vue";
import EventDetails from "./components/EventDetails.vue";
import Confirmation from "./components/Confirmation.vue";

export default {
  setup() {
    const step = ref(1);
    const personalInfo = ref({
      lastName: "",
      firstName: "",
      email: "",
      phone: "",
      postalCode: "",
    });
    const events = ref(null);
    const selectedEvent = ref(null);
    const selectedSession = ref(null);

    const currentComponent = computed(() => {
      if (step.value === 1) return SignupForm;
      if (step.value === 2) return EventList;
      if (step.value === 3) return EventDetails;
      if (step.value === 4) return Confirmation;
      return null;
    });

    const nextStep = (data) => {
      if (step.value === 1) {
        personalInfo.value = data;
        step.value++;
      } else if (step.value === 2) {
        selectedEvent.value = data;
        step.value++;
      } else if (step.value === 3) {
        selectedSession.value = data;
        step.value++;
      }
    };

    const goBack = () => {
      step.value--;
    };

    const goToStep = (stepNumber) => {
      step.value = stepNumber;
    };

    const updatePersonalInfo = (newPersonalInfo) => {
      personalInfo.value = newPersonalInfo;
    };

    return {
      step,
      personalInfo,
      events,
      selectedEvent,
      selectedSession,
      currentComponent,
      nextStep,
      goBack,
      goToStep,
      updatePersonalInfo,
    };
  },
};
</script>

<style>
.progress-bar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  cursor: pointer;
}

.progress-bar div {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: lightgray;
  display: flex;
  align-items: center;
  justify-content: center;
}

.progress-bar .active {
  background-color: blue;
  color: white;
}
</style>
