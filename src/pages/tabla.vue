<template>
    <v-card
      hover
      title="To Dos"
    >
      <template v-slot:text>
        <v-text-field
          v-model="search"
          label="Search"
          prepend-inner-icon="mdi-magnify"
          single-line
          variant="outlined"
          hide-details
        ></v-text-field>
      </template>
  
      <v-data-table density="compact" color="teal-lighten-4"
        :headers="headers"
        :items="datos"
        :search="search"
      ></v-data-table>
    </v-card>
  </template>

  <script setup>
  import { ref } from 'vue';

  const num = ref(1);
  const search = ref('');
  const headers = ref(
    [
            {
              align: 'start',
              key: 'name',
              sortable: false,
              title: 'ToDos',
            },
            { key: 'userId', title: 'User ID' },
            { key: 'id', title: 'ID' },
            { key: 'title', title: 'Title' },
            { key: 'completed', title: 'Completed' },
          ]

  );

const datos = ref([]);

async function fetchToDos() {
  try {
    num.value = Math.floor(Math.random() * 199 + 1);
    const response = await fetch(`https://jsonplaceholder.typicode.com/todos/${num.value}`);
    const data = await response.json();
    datos.value.push(data);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}

for(let i = 0; i < 8; i++) {
    fetchToDos();
}
    

  </script>