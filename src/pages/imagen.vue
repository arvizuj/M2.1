<script setup>

import { ref } from 'vue'

const num_imagen = ref(93)
const chiste = ref('')
const show = ref(false)

async function fetchChuckNorrisJoke() {
    try {
        const response = await fetch('https://api.chucknorris.io/jokes/random');
        const data = await response.json();
        return data.value;
    } catch (error) {
        console.error('Error fetching Chuck Norris joke:', error);
        return null;
    }
}
fetchChuckNorrisJoke().then(joke => {
    if (joke) {
        chiste.value = joke;
    } else {
        chiste.value = 'No se pudo obtener la broma de Chuck Norris.';
    }
});
</script>

<template>
  <v-card
    class="mx-auto"
    max-width="500"
    hover
   >
    <v-img
        img :src="`https://picsum.photos/id/${num_imagen}/500/300`" 
        height="300px"
        width="500px"
        cover>
        <template v-slot:error>
          <v-img
            class="mx-auto"
            height="300"
            max-width="500"
            src="https://picsum.photos/id/93/500/300"
          ></v-img>
        </template>
        <template v-slot:placeholder>
          <div class="d-flex align-center justify-center fill-height">
            <v-progress-circular
              color="grey-lighten-4"
              indeterminate
            ></v-progress-circular>
          </div>
        </template>
    </v-img>
    <v-card-title>
      Imagen obtenida
    </v-card-title>
    <v-card-subtitle>
      Presione para obtener otra imagen
    </v-card-subtitle>
    
    <v-card-actions>
      <v-btn
        color="pink-lighten-2"
        variant="text"
        @click="num_imagen = Math.floor(Math.random() * 1085)"
      >
        Generar nueva imagen
      </v-btn>

      <v-spacer></v-spacer>

      <v-btn
        :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'"
        @click="show = !show"
      ></v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
        <v-card-text>
          {{chiste}}
        </v-card-text>
      </div>
    </v-expand-transition>
    
  </v-card>
  
</template>