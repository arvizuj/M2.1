<template>
  <v-card max-width="1000px" hover title="Users" color="pink-lighten-5">
    <template v-slot:text>
      <v-text-field 
        v-model="search" 
        label="Search" 
        prepend-inner-icon="mdi-magnify" 
        single-line variant="outlined"
        hide-details>
      </v-text-field>
    </template>

    <v-container align="center" justify="center">
      <v-row align="center" justify="center">
        <v-col cols="auto">
          <v-btn 
            elevation="4" 
            density="comfortable"
            color="light-green-lighten-3" @click="showNewDialog = true">
            New Item
          </v-btn>
        </v-col>
        <v-col cols="auto">
          <v-btn 
            elevation="4" 
            density="comfortable" 
            color="amber-lighten-4" @click="showUpdateDialog = true">
            Update Item
          </v-btn>
        </v-col>
        <v-col cols="auto">
          <v-btn 
            elevation="4" 
            density="comfortable" 
            color="red-lighten-3" @click="showDeleteDialog = true">
            Delete Item
          </v-btn>
        </v-col>
      </v-row>

    </v-container>

    <v-data-table density="compact" 
      :headers="headers" 
      :items="datos" 
      :search="search"
      :sort-by="[{ key: 'id', order: 'asc' }]" :sort-by1="[{ key: 'title', order: 'asc' }]"
      :sort-by2="[{ key: 'body', order: 'asc' }]"></v-data-table>


    <v-dialog v-model="showDeleteDialog" max-width="500px">
      <v-card color="red-lighten-5">
        <v-card-title>Delete Item</v-card-title>
        <v-card-text>
          <v-text-field 
            v-model.number="idToDelete" 
            label="ID to delete" 
            type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn 
            color="red darken-1" 
            text @click="eliminar(idToDelete); 
            showDeleteDialog = false">
            Delete
          </v-btn>
          <v-btn 
            color="red-darken-4" 
            text @click="showDeleteDialog = false">
            Cancel
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>


    <v-dialog v-model="showNewDialog" max-width="500px">
      <v-card color="green-lighten-5">
        <v-card-title>New Item</v-card-title>
        <v-card-text>
          <v-text-field v-model="newTitle" label="Title"></v-text-field>
          <v-text-field v-model="newBody" label="Body"></v-text-field>
          <v-text-field v-model.number="newUserId" label="User ID" type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="light-green-darken-3" @click="crearNuevoItem">Create</v-btn>
          <v-btn color="red-darken-4" text @click="showNewDialog = false">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>


    <v-dialog v-model="showUpdateDialog" max-width="500px">
      <v-card color="amber-lighten-5">
        <v-card-title>Update Item</v-card-title>
        <v-card-text>
          <v-text-field v-model.number="updateId" label="ID to update" type="number"></v-text-field>
          <v-text-field v-model="updateTitle" label="New Title"></v-text-field>
          <v-text-field v-model="updateBody" label="New Body"></v-text-field>
          <v-text-field v-model.number="updateUserId" label="New User ID" type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="amber-accent-4" @click="actualizarElemento">Update</v-btn>
          <v-btn color="red-darken-4" text @click="showUpdateDialog = false">Cancel</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  </v-card>
</template>

<script setup>
import { ref } from 'vue';

const search = ref('');
const headers = ref([
  { align: 'start', key: 'userId', sortable: false, title: 'User ID' },
  { key: 'id', title: 'ID' },
  { key: 'title', title: 'Title' },
  { key: 'body', title: 'Body' }
]);

const datos = ref([]);

//Funciones para crear un nuevo registro
const showNewDialog = ref(false);
const newTitle = ref('');
const newBody = ref('');
const newUserId = ref('');

//Funciones para actualizar un registro
const showUpdateDialog = ref(false);
const updateId = ref('');
const updateTitle = ref('');
const updateBody = ref('');
const updateUserId = ref('');

//Funciones para borrar un registro
const showDeleteDialog = ref(false);
const idToDelete = ref('');

async function obtenerDatos() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    const data = await response.json();
    datos.value = data;
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
}

async function crearNuevoItem() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      body: JSON.stringify({
        title: newTitle.value,
        body: newBody.value,
        userId: newUserId.value,
      }),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    });

    if (response.ok) {
      const data = await response.json();
      console.log(data);
      datos.value.push(data);
      showNewDialog.value = false;
    } else {
      throw new Error('Hubo un problema al crear el nuevo post.');
    }

  } catch (error) {
    console.error('Error al crear el nuevo post:', error);
  }
}

async function actualizarElemento() {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${updateId.value}`, {
      method: 'PUT',
      body: JSON.stringify({
        id: updateId.value,
        title: updateTitle.value,
        body: updateBody.value,
        userId: updateUserId.value,
      }),
      headers: {
        'Content-type': 'application/json; charset=UTF-8',
      },
    });

    if (response.ok) {
      const updatedData = await response.json();
      console.log(updatedData);

      const index = datos.value.findIndex(post => post.id === updateId.value);
      if (index !== -1) {
        datos.value[index] = updatedData;
      }

      showUpdateDialog.value = false;
    } else {
      throw new Error('Hubo un problema al actualizar el post.');
    }

  } catch (error) {
    console.error('Error al actualizar el post:', error);
  }
}

async function eliminar(id) {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
      method: 'DELETE',
    });

    const data = await response.json();
    console.log(data);
    const index = datos.value.findIndex(post => post.id === id);
    if (index !== -1) {
      datos.value.splice(index, 1);
    }

  } catch (error) {
    console.error('Error al crear el delete:', error);
  }
}

obtenerDatos();
</script>
