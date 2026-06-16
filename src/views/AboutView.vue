<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center px-4">
    <div class="max-w-2xl bg-white rounded-2xl shadow-xl p-8 text-center">
      <h1 class="text-4xl font-bold text-gray-800 mb-4">Acerca de</h1>
      <p class="text-gray-600 text-lg">
        Esta es una aplicación de gestión de productos construida con
        <strong>Vue.js</strong> y <strong>FastAPI</strong>.
      </p>
      <p class="text-gray-500 mt-2">
        Desplegada en Netlify (frontend) y Render (backend) con base de datos en Neon.
      </p>
      <div class="mt-6">
        <router-link
          to="/"
          class="inline-block px-6 py-2 bg-green-500 text-white rounded-lg hover:bg-green-600 transition"
        >
          ← Volver a productos
        </router-link>
      </div>
    </div>
  </div>
</template>