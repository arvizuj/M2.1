<script setup>
</script>

<template>
  <v-card>
    <v-layout>
      <v-app-bar color="pink-lighten-2" title="CRUD con Vuetify"></v-app-bar>
      <v-navigation-drawer
        color="pink-lighten-1"
        v-model="drawer"
        :rail="rail"
        permanent
        @click="rail = false"
      >
        <v-list-item
          prepend-icon="mdi-menu"
          title="Menu"
          nav
        >
          <template v-slot:append>
            <v-btn
              variant="text"
              icon="mdi-chevron-left"
              @click.stop="rail = !rail"
            ></v-btn>
          </template>
        </v-list-item>

        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-home-city" link to="/" title="Inicio"></v-list-item>
          <v-list-item prepend-icon="mdi-image" link to="/imagen" title="Imagen y chiste"></v-list-item>
          <v-list-item prepend-icon="mdi-table" link to="/tabla" title="Tabla"></v-list-item>
          <v-list-item prepend-icon="mdi-table" link to="/tabla_2" title="Tabla con CRUD"></v-list-item>
        </v-list>
      </v-navigation-drawer>
      
      <v-main class="d-flex align-center justify-center" style="min-height: 730px;"><router-view></router-view></v-main>
    </v-layout>
  </v-card>
</template>
<script>
  export default {
    data () {
      return {
        drawer: true,
        rail: true,
      }
    },
  }
</script>


