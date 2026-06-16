<template>
  <div class="min-h-screen bg-gray-100 py-12 px-4">
    <div class="max-w-3xl mx-auto bg-white rounded-2xl shadow-xl p-8">
      <h1 class="text-3xl font-bold text-center text-gray-800 mb-4">
        Acerca de
      </h1>
      
      <p class="text-gray-600 text-center mb-8">
        Aplicacion de gestion de productos construida con Vue.js y FastAPI.
        Desplegada en Netlify y Render con base de datos en Neon.
      </p>

      <!-- Frontend -->
      <div class="border-t border-gray-200 pt-4 pb-2">
        <h2 class="text-lg font-semibold text-green-600 mb-2">Frontend</h2>
        <div class="grid grid-cols-2 gap-2 pl-4">
          <div><span class="text-gray-600">Framework:</span> Vue.js 3</div>
          <div><span class="text-gray-600">Lenguaje:</span> JavaScript</div>
          <div><span class="text-gray-600">CSS:</span> Tailwind CSS</div>
          <div><span class="text-gray-600">Hosting:</span> Netlify</div>
        </div>
      </div>

      <!-- Backend -->
      <div class="border-t border-gray-200 pt-4 pb-2">
        <h2 class="text-lg font-semibold text-blue-600 mb-2">Backend</h2>
        <div class="grid grid-cols-2 gap-2 pl-4">
          <div><span class="text-gray-600">Framework:</span> FastAPI</div>
          <div><span class="text-gray-600">Lenguaje:</span> Python</div>
          <div><span class="text-gray-600">ORM:</span> SQLAlchemy</div>
          <div><span class="text-gray-600">Hosting:</span> Render</div>
        </div>
      </div>

      <!-- Base de datos -->
      <div class="border-t border-gray-200 pt-4 pb-2">
        <h2 class="text-lg font-semibold text-purple-600 mb-2">Base de datos</h2>
        <div class="grid grid-cols-2 gap-2 pl-4">
          <div><span class="text-gray-600">Motor:</span> PostgreSQL</div>
          <div><span class="text-gray-600">Nube:</span> Neon</div>
        </div>
      </div>

      <!-- Desarrollo -->
      <div class="border-t border-gray-200 pt-4 pb-2">
        <h2 class="text-lg font-semibold text-gray-700 mb-2">Desarrollo</h2>
        <div class="grid grid-cols-2 gap-2 pl-4">
          <div><span class="text-gray-600">Control versiones:</span> Git + GitHub</div>
          <div><span class="text-gray-600">Despliegue:</span> CI/CD automatico</div>
        </div>
      </div>

      <!-- Resumen visual -->
      <div class="border-t border-gray-200 mt-4 pt-4">
        <h2 class="text-lg font-semibold text-gray-700 mb-2 text-center">Stack completo</h2>
        <div class="flex flex-col items-center text-sm text-gray-600 space-y-1">
          <div>Frontend: Vue.js 3 → Netlify</div>
          <div>Backend: FastAPI → Render</div>
          <div>Base de datos: PostgreSQL → Neon</div>
        </div>
      </div>

      <div class="mt-8 text-center">
        <router-link
          to="/"
          class="inline-block px-6 py-2 bg-green-500 text-white rounded-lg hover:bg-green-600 transition"
        >
          Volver a productos
        </router-link>
      </div>
    </div>
  </div>
</template>

<script setup>
import { RouterLink } from 'vue-router'
</script>