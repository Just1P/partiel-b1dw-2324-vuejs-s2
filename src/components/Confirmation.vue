<template>
  <div class="confirmation-container">
    <h2>Confirmation de l'Inscription</h2>
    <div ref="confirmationContent" class="section personal-info">
      <h3>Informations Personnelles</h3>
      <p><strong>Nom:</strong> {{ personalInfo.lastName }}</p>
      <p><strong>Prénom:</strong> {{ personalInfo.firstName }}</p>
      <p><strong>Email:</strong> {{ personalInfo.email }}</p>
      <p><strong>Téléphone:</strong> {{ personalInfo.phone }}</p>
      <p><strong>Code Postal:</strong> {{ personalInfo.postalCode }}</p>
      <button @click="editPersonalInfo" class="edit-button">Modifier</button>
    </div>
    <div class="section session-details">
      <h3>Détails de la Session</h3>
      <p><strong>Nom de l'Événement:</strong> {{ event.name }}</p>
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
    <button @click="downloadPdf" class="download-button">
      Télécharger en PDF
    </button>

    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <h3>Modifier les Informations Personnelles</h3>
        <form @submit.prevent="savePersonalInfo">
          <div class="form-group">
            <label for="lastName">Nom:</label>
            <input
              type="text"
              v-model="editedPersonalInfo.lastName"
              id="lastName"
              required
            />
          </div>
          <div class="form-group">
            <label for="firstName">Prénom:</label>
            <input
              type="text"
              v-model="editedPersonalInfo.firstName"
              id="firstName"
              required
            />
          </div>
          <div class="form-group">
            <label for="email">Email:</label>
            <input
              type="email"
              v-model="editedPersonalInfo.email"
              id="email"
              required
            />
          </div>
          <div class="form-group">
            <label for="phone">Téléphone:</label>
            <input
              type="tel"
              v-model="editedPersonalInfo.phone"
              id="phone"
              required
              pattern="[0-9]{10}"
            />
          </div>
          <div class="form-group">
            <label for="postalCode">Code Postal:</label>
            <input
              type="text"
              v-model="editedPersonalInfo.postalCode"
              id="postalCode"
              required
              pattern="[0-9]{5}"
            />
          </div>
          <button type="submit" class="save-button">Enregistrer</button>
          <button type="button" @click="closeModal" class="cancel-button">
            Annuler
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import jsPDF from "jspdf";
import { ref, watch } from "vue";

export default {
  props: ["personalInfo", "event", "session"],
  setup(props, { emit }) {
    const showModal = ref(false);
    const editedPersonalInfo = ref({ ...props.personalInfo });

    watch(props.personalInfo, (newVal) => {
      editedPersonalInfo.value = { ...newVal };
    });

    const editPersonalInfo = () => {
      showModal.value = true;
    };

    const closeModal = () => {
      showModal.value = false;
    };

    const savePersonalInfo = () => {
      emit("update-personal-info", editedPersonalInfo.value);
      closeModal();
    };

    const downloadPdf = () => {
      const doc = new jsPDF();

      doc.text("Confirmation de l'Inscription", 10, 10);

      doc.text("Informations Personnelles", 10, 20);
      doc.text(`Nom: ${props.personalInfo.lastName}`, 10, 30);
      doc.text(`Prénom: ${props.personalInfo.firstName}`, 10, 40);
      doc.text(`Email: ${props.personalInfo.email}`, 10, 50);
      doc.text(`Téléphone: ${props.personalInfo.phone}`, 10, 60);
      doc.text(`Code Postal: ${props.personalInfo.postalCode}`, 10, 70);

      doc.text("Détails de la Session", 10, 90);
      doc.text(`Nom de l'Événement: ${props.event.name}`, 10, 100);
      doc.text(`Option: ${props.session.option}`, 10, 110);
      doc.text(`Catégorie: ${props.session.categories}`, 10, 120);
      doc.text(`Date: ${props.session.date}`, 10, 130);
      doc.text(
        `Heure: ${props.session.start_at} - ${props.session.end_at}`,
        10,
        140
      );
      doc.text(
        `Places restantes: ${
          props.session.available_places - props.session.registered
        }`,
        10,
        150
      );

      doc.save("confirmation.pdf");
    };

    return {
      showModal,
      editedPersonalInfo,
      editPersonalInfo,
      closeModal,
      savePersonalInfo,
      downloadPdf,
    };
  },
};
</script>

<style scoped>
.confirmation-container {
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

.section {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 20px;
}

h3 {
  color: #666;
  font-size: 1.2em;
  margin-bottom: 10px;
}

p {
  margin: 5px 0;
}

.download-button,
.edit-button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s ease;
}

.download-button:hover,
.edit-button:hover {
  background-color: #0056b3;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  width: 300px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
  display: block;
  color: #666;
}

input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

input:focus {
  border-color: #007bff;
  outline: none;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

.save-button,
.cancel-button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s ease;
}

.save-button:hover {
  background-color: #0056b3;
}

.cancel-button {
  background-color: #6c757d;
  margin-left: 10px;
}

.cancel-button:hover {
  background-color: #5a6268;
}
</style>
