<template>
  <div>
    <!-- Barra de navegación -->
    <nav class="bg-white shadow-md py-3 px-6 flex justify-between items-center">
      <router-link to="/" class="text-xl font-bold text-green-600">
        Productos
      </router-link>
      <router-link to="/about" class="text-gray-600 hover:text-green-600 transition">
        Acerca de
      </router-link>
    </nav>
    
    <!-- Contenido principal -->
    <router-view />
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>