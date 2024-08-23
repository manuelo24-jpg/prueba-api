<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const data = ref([]);
const errorMessage = ref("");
const selectedItem = ref(null);

const options = {
  method: "GET",
  url: "https://instagram243.p.rapidapi.com/searchlocation/paris",
  headers: {
    "x-rapidapi-key": "7857f3e008mshdbc29ebea76e69ep15fe69jsn9484898f9ab3",
    "x-rapidapi-host": "instagram243.p.rapidapi.com",
  },
};

onMounted(async () => {
  try {
    const response = await axios.request(options);
    console.log(response.data);
    data.value = response.data.data;
  } catch (error) {
    console.error(error);
    errorMessage.value = error.message;
  }
});

const selectItem = (item) => {
  selectedItem.value = item;
};
</script>
<template>
  <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  <table v-else class="styled-table">
    <thead>
      <tr>
        <th>Título</th>
        <th>Subtítulo</th>
        <th>Nombre</th>
        <th>Ciudad</th>
        <th>Latitud</th>
        <th>Longitud</th>
        <th>Detalles</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in data" :key="item.location.pk">
        <td>{{ item.title }}</td>
        <td>{{ item.subtitle }}</td>
        <td>{{ item.location.name }}</td>
        <td>{{ item.location.city }}</td>
        <td>{{ item.location.lat }}</td>
        <td>{{ item.location.lng }}</td>
        <td><button @click="selectItem(item)">Ver Detalles</button></td>
      </tr>
    </tbody>
  </table>
  <div v-if="selectedItem" class="details">
    <h3>Detalles del Elemento Seleccionado</h3>
    <p><strong>Título:</strong> {{ selectedItem.title }}</p>
    <p><strong>Subtítulo:</strong> {{ selectedItem.subtitle }}</p>
    <p><strong>Nombre:</strong> {{ selectedItem.location.name }}</p>
    <p><strong>Ciudad:</strong> {{ selectedItem.location.city }}</p>
    <p><strong>Latitud:</strong> {{ selectedItem.location.lat }}</p>
    <p><strong>Longitud:</strong> {{ selectedItem.location.lng }}</p>
  </div>
</template>
<style scoped>
.styled-table {
  width: 100%;
  border-collapse: collapse;
  margin: 25px 0;
  font-size: 18px;
  text-align: left;
}
.styled-table thead tr {
  background-color: #009879;
  color: #ffffff;
  text-align: left;
}
.styled-table th,
.styled-table td {
  padding: 12px 15px;
}
.styled-table tbody tr {
  border-bottom: 1px solid #dddddd;
}
.styled-table tbody tr:nth-of-type(even) {
  background-color: #f3f3f3;
}
.styled-table tbody tr:last-of-type {
  border-bottom: 2px solid #009879;
}
.details-button {
  background-color: #009879;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
  margin-left: 10px;
}
.details-button:hover {
  background-color: #007f63;
}
.error-message {
  color: red;
  font-weight: bold;
}
.details {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #dddddd;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.details h3 {
  margin-top: 0;
  color: #009879;
}
.details p {
  margin: 5px 0;
}
</style>
