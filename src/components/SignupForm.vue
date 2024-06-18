<template>
  <div class="signup-form-container">
    <h2>Informations Personnelles</h2>
    <form @submit.prevent="submitForm" class="signup-form">
      <div class="form-group">
        <label for="lastName">Nom:</label>
        <input type="text" v-model="lastName" id="lastName" required />
      </div>
      <div class="form-group">
        <label for="firstName">Prénom:</label>
        <input type="text" v-model="firstName" id="firstName" required />
      </div>
      <div class="form-group">
        <label for="email">Email:</label>
        <input type="email" v-model="email" id="email" required />
      </div>
      <div class="form-group">
        <label for="phone">Téléphone:</label>
        <input type="tel" v-model="phone" id="phone" required />
        <span v-if="phoneError" class="error-message">{{ phoneError }}</span>
      </div>
      <div class="form-group">
        <label for="postalCode">Code Postal:</label>
        <input type="text" v-model="postalCode" id="postalCode" required />
        <span v-if="postalCodeError" class="error-message">{{
          postalCodeError
        }}</span>
      </div>
      <button type="submit" class="submit-button">Suivant</button>
    </form>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup(props, { emit }) {
    const lastName = ref("");
    const firstName = ref("");
    const email = ref("");
    const phone = ref("");
    const postalCode = ref("");
    const phoneError = ref(null);
    const postalCodeError = ref(null);

    const validatePhone = () => {
      phoneError.value =
        phone.value.length !== 10
          ? "Le numéro de téléphone doit comporter 10 chiffres."
          : null;
    };

    const validatePostalCode = () => {
      postalCodeError.value =
        postalCode.value.length !== 5
          ? "Le code postal doit comporter 5 chiffres."
          : null;
    };

    const submitForm = () => {
      validatePhone();
      validatePostalCode();
      if (!phoneError.value && !postalCodeError.value) {
        emit("next-step", {
          lastName: lastName.value,
          firstName: firstName.value,
          email: email.value,
          phone: phone.value,
          postalCode: postalCode.value,
        });
      }
    };

    return {
      lastName,
      firstName,
      email,
      phone,
      postalCode,
      phoneError,
      postalCodeError,
      validatePhone,
      validatePostalCode,
      submitForm,
    };
  },
};
</script>

<style scoped>
.signup-form-container {
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

.signup-form {
  display: flex;
  flex-direction: column;
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

.submit-button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s ease;
}

.submit-button:hover {
  background-color: #0056b3;
}

.error-message {
  color: red;
  font-size: 0.9em;
  margin-top: 5px;
}
</style>
