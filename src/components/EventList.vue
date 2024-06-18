<template>
  <div class="event-list-container">
    <h2>Événements Disponibles</h2>
    <div class="filter-container">
      <label for="category">Choisir une catégorie :</label>
      <select id="category" v-model="selectedCategory" class="category-select">
        <option value="" disabled>-- Sélectionnez une catégorie --</option>
        <option value="Children">Enfants</option>
        <option value="Adults">Adultes</option>
      </select>
    </div>
    <div v-if="selectedCategory">
      <div
        class="event-card"
        v-for="event in filteredEvents"
        :key="event.name"
        @click="selectEvent(event)"
      >
        <div class="event-header">
          <div>
            <h3>{{ event.name }}</h3>
            <p class="event-categories">{{ event.categories.join(", ") }}</p>
          </div>
        </div>
        <div class="event-details">
          <p><strong>Dates Disponibles:</strong></p>
          <ul>
            <li
              v-for="session in event.sessions"
              :key="session.date + session.start_at"
            >
              {{ session.date }}
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div v-else>
      <p>
        Veuillez sélectionner une catégorie pour voir les événements
        disponibles.
      </p>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";

export default {
  setup(props, { emit }) {
    const events = ref([]);
    const selectedCategory = ref("");

    onMounted(async () => {
      const response = await fetch("/data_activities.json");
      events.value = await response.json();
    });

    const filteredEvents = computed(() => {
      if (!selectedCategory.value) return [];
      return events.value.filter((event) =>
        event.categories.includes(selectedCategory.value)
      );
    });

    const selectEvent = (event) => {
      emit("next-step", event);
    };

    return {
      events,
      selectedCategory,
      filteredEvents,
      selectEvent,
    };
  },
};
</script>

<style scoped>
.event-list-container {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #333;
  font-size: 1.5em;
  margin-bottom: 20px;
}

.filter-container {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-right: 10px;
  color: #666;
}

.category-select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.event-card {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 8px;
  margin-bottom: 15px;
  padding: 15px;
  display: flex;
  flex-direction: column;
  cursor: pointer;
  transition: box-shadow 0.3s ease;
}

.event-card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.event-header {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
}

.event-icon {
  width: 50px;
  height: 50px;
  margin-right: 15px;
}

.event-details {
  margin-top: 10px;
}

.event-categories {
  color: #666;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #f1f1f1;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin: 5px 0;
  padding: 8px;
}
</style>
