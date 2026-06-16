<template>
  <div class="min-h-screen bg-gray-100 py-12 px-4">
    <div class="max-w-3xl mx-auto bg-white rounded-2xl shadow-xl p-8">
      <h1 class="text-3xl font-bold text-center text-gray-800 mb-6">
        Acerca de
      </h1>
      
      <p class="text-gray-600 text-center mb-8">
        Aplicacion de gestion de productos construida con Vue.js y FastAPI.
        Desplegada en Netlify y Render con base de datos en Neon.
      </p>

      <div class="border-t border-gray-200 pt-6">
        <h2 class="text-xl font-semibold text-gray-700 mb-4">Tecnologias utilizadas</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Frontend:</span>
            <span class="text-gray-800">Vue.js 3</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Lenguaje:</span>
            <span class="text-gray-800">JavaScript</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Framework CSS:</span>
            <span class="text-gray-800">Tailwind CSS</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Backend:</span>
            <span class="text-gray-800">FastAPI</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">ORM:</span>
            <span class="text-gray-800">SQLAlchemy</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Base de datos:</span>
            <span class="text-gray-800">PostgreSQL</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">DB en la nube:</span>
            <span class="text-gray-800">Neon</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Backend hosting:</span>
            <span class="text-gray-800">Render</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Frontend hosting:</span>
            <span class="text-gray-800">Netlify</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="font-medium text-gray-600">Control de versiones:</span>
            <span class="text-gray-800">Git + GitHub</span>
          </div>
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