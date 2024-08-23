<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const data = ref([]);
const errorMessage = ref("");

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
      </tr>
    </tbody>
  </table>
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
.error-message {
  color: red;
  font-weight: bold;
}
</style>
