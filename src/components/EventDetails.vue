<template>
  <div class="event-details-container">
    <h2>Détails de l'Événement: {{ event.name }}</h2>
    <div
      v-for="session in event.sessions"
      :key="session.date + session.start_at"
      class="session-card"
      @click="selectSession(session)"
    >
      <div class="session-details">
        <p><strong>Option:</strong> {{ session.option }}</p>
        <p><strong>Catégorie:</strong> {{ session.categories }}</p>
        <p><strong>Date:</strong> {{ session.date }}</p>
        <p>
          <strong>Heure:</strong> {{ session.start_at }} - {{ session.end_at }}
        </p>
        <p>
          <strong>Places restantes:</strong>
          {{ session.available_places - session.registered }}
        </p>
      </div>
    </div>
    <button @click="goBack" class="back-button">Retour</button>
  </div>
</template>

<script>
export default {
  props: ["event"],
  setup(props, { emit }) {
    const selectSession = (session) => {
      emit("next-step", session);
    };

    const goBack = () => {
      emit("go-back");
    };

    return {
      selectSession,
      goBack,
    };
  },
};
</script>

<style scoped>
.event-details-container {
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

.session-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  margin: 16px 0;
  padding: 16px;
  background-color: #fff;
  cursor: pointer;
  transition: box-shadow 0.3s ease;
}

.session-card:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.session-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
}

.session-details p {
  margin: 5px 0;
}

button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

.back-button {
  margin-top: 20px;
}
</style>
