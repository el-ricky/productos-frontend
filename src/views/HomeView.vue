<template>
  <div class="min-h-screen bg-gray-100 py-8 px-4">
    <div class="max-w-5xl mx-auto">
      <!-- Título -->
      <h1 class="text-4xl font-bold text-center text-gray-800 mb-8">
        Productos
      </h1>

      <!-- Formulario -->
      <div class="bg-white rounded-xl shadow-md p-6 mb-8">
        <div class="flex flex-col sm:flex-row gap-4">
          <input
            v-model="nuevoProducto.nombre"
            placeholder="Nombre del producto"
            class="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-400"
          />
          <input
            v-model.number="nuevoProducto.precio"
            placeholder="Precio"
            type="number"
            class="w-full sm:w-40 px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-400"
          />
          <button
            @click="agregarProducto"
            class="px-6 py-2 bg-green-500 text-white font-semibold rounded-lg hover:bg-green-600 transition"
          >
            Agregar
          </button>
        </div>
      </div>

      <!-- Tabla -->
      <div class="bg-white rounded-xl shadow-md overflow-hidden">
        <table class="w-full text-left">
          <thead class="bg-gray-200 text-gray-700">
            <tr>
              <th class="px-6 py-3 text-sm font-semibold">ID</th>
              <th class="px-6 py-3 text-sm font-semibold">Nombre</th>
              <th class="px-6 py-3 text-sm font-semibold">Precio</th>
              <th class="px-6 py-3 text-sm font-semibold text-center">Acciones</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200">
            <tr v-for="producto in productos" :key="producto.id" class="hover:bg-gray-50">
              <td class="px-6 py-4 text-sm">{{ producto.id }}</td>
              <td class="px-6 py-4 text-sm">
                <span v-if="editando !== producto.id">{{ producto.nombre }}</span>
                <input v-else v-model="editProducto.nombre" class="border px-2 py-1 rounded" />
              </td>
              <td class="px-6 py-4 text-sm">
                <span v-if="editando !== producto.id">${{ producto.precio }}</span>
                <input v-else v-model.number="editProducto.precio" type="number" class="border px-2 py-1 rounded w-24" />
              </td>
              <td class="px-6 py-4 text-sm text-center space-x-2">
                <!-- Botón Editar / Guardar -->
                <button
                  v-if="editando !== producto.id"
                  @click="editarProducto(producto)"
                  class="px-3 py-1 bg-yellow-400 text-white rounded hover:bg-yellow-500 transition"
                >
                  Editar
                </button>
                <button
                  v-else
                  @click="guardarEdicion(producto.id)"
                  class="px-3 py-1 bg-blue-500 text-white rounded hover:bg-blue-600 transition"
                >
                  Guardar
                </button>

                <!-- Botón Eliminar -->
                <button
                  @click="eliminarProducto(producto.id)"
                  class="px-3 py-1 bg-red-500 text-white rounded hover:bg-red-600 transition"
                >
                  Eliminar
                </button>

                <!-- Botón Cancelar (solo en edición) -->
                <button
                  v-if="editando === producto.id"
                  @click="cancelarEdicion"
                  class="px-3 py-1 bg-gray-400 text-white rounded hover:bg-gray-500 transition"
                >
                  Cancelar
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const productos = ref([])
const nuevoProducto = ref({ nombre: '', precio: 0 })
const editando = ref(null)
const editProducto = ref({ nombre: '', precio: 0 })

const API_URL = 'https://productos-backend-wcpx.onrender.com/productos'

const cargarProductos = async () => {
  try {
    const res = await axios.get(API_URL)
    productos.value = res.data
  } catch (error) {
    console.error('Error al cargar productos:', error)
  }
}

const agregarProducto = async () => {
  if (!nuevoProducto.value.nombre || nuevoProducto.value.precio <= 0) {
    alert('Nombre y precio válido son obligatorios')
    return
  }
  try {
    await axios.post(API_URL, nuevoProducto.value)
    nuevoProducto.value = { nombre: '', precio: 0 }
    cargarProductos()
  } catch (error) {
    console.error('Error al agregar producto:', error)
  }
}

const editarProducto = (producto) => {
  editando.value = producto.id
  editProducto.value = { nombre: producto.nombre, precio: producto.precio }
}

const guardarEdicion = async (id) => {
  try {
    await axios.put(`${API_URL}/${id}`, editProducto.value)
    editando.value = null
    cargarProductos()
  } catch (error) {
    console.error('Error al editar producto:', error)
  }
}

const cancelarEdicion = () => {
  editando.value = null
}

const eliminarProducto = async (id) => {
  if (!confirm('¿Eliminar este producto?')) return
  try {
    await axios.delete(`${API_URL}/${id}`)
    cargarProductos()
  } catch (error) {
    console.error('Error al eliminar producto:', error)
  }
}

onMounted(cargarProductos)
</script>