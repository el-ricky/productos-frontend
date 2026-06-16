<template>
  <div class="min-h-screen bg-gray-100 py-8 px-4">
    <div class="max-w-5xl mx-auto">
      <!-- Titulo -->
      <h1 class="text-4xl font-bold text-center text-gray-800 mb-8">
        Productos
      </h1>

      <!-- Buscador -->
      <div class="mb-4">
        <input
          v-model="filtro"
          type="text"
          placeholder="Buscar producto por nombre..."
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-400"
        />
      </div>

      <!-- Formulario mejorado -->
      <div class="bg-white rounded-xl shadow-md p-6 mb-8">
        <h2 class="text-lg font-semibold text-gray-700 mb-4">Agregar nuevo producto</h2>
        <div class="flex flex-col sm:flex-row gap-4">
          <div class="flex-1">
            <label class="block text-sm font-medium text-gray-600 mb-1">Nombre</label>
            <input
              v-model="nuevoProducto.nombre"
              placeholder="Ej: Laptop, Mouse, Teclado..."
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-400"
            />
          </div>
          <div class="w-full sm:w-48">
            <label class="block text-sm font-medium text-gray-600 mb-1">Precio (MXN)</label>
            <input
              v-model.number="nuevoProducto.precio"
              placeholder="0.00"
              type="number"
              step="0.01"
              min="0"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-400"
            />
          </div>
          <div class="flex items-end">
            <button
              @click="agregarProducto"
              class="w-full sm:w-auto px-6 py-2 bg-green-500 text-white font-semibold rounded-lg hover:bg-green-600 transition"
            >
              Agregar producto
            </button>
          </div>
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
            <tr v-for="producto in productosPaginados" :key="producto.id" class="hover:bg-gray-50">
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
                <!-- Boton Editar / Guardar -->
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

                <!-- Boton Eliminar -->
                <button
                  @click="eliminarProducto(producto.id)"
                  class="px-3 py-1 bg-red-500 text-white rounded hover:bg-red-600 transition"
                >
                  Eliminar
                </button>

                <!-- Boton Cancelar (solo en edicion) -->
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
        
        <!-- Paginacion -->
        <div class="flex justify-between items-center mt-4 px-6 py-3 bg-gray-50">
          <button
            @click="paginaAnterior"
            :disabled="paginaActual === 1"
            class="px-4 py-2 bg-gray-300 text-gray-700 rounded-lg disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-400 transition"
          >
            Anterior
          </button>
          
          <span class="text-gray-600">
            Pagina {{ paginaActual }} de {{ totalPaginas }}
          </span>
          
          <button
            @click="paginaSiguiente"
            :disabled="paginaActual === totalPaginas"
            class="px-4 py-2 bg-gray-300 text-gray-700 rounded-lg disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-400 transition"
          >
            Siguiente
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import axios from 'axios'
import { toast } from 'vue3-toastify'

const productos = ref([])
const nuevoProducto = ref({ nombre: '', precio: 0 })
const editando = ref(null)
const editProducto = ref({ nombre: '', precio: 0 })

const paginaActual = ref(1)
const productosPorPagina = 5
const filtro = ref('')

// Productos filtrados por busqueda
const productosFiltrados = computed(() => {
  if (!filtro.value.trim()) {
    return productos.value
  }
  const busqueda = filtro.value.toLowerCase().trim()
  return productos.value.filter(p => 
    p.nombre.toLowerCase().includes(busqueda)
  )
})

// Total de paginas basado en productos filtrados
const totalPaginas = computed(() => {
  return Math.ceil(productosFiltrados.value.length / productosPorPagina)
})

// Productos paginados
const productosPaginados = computed(() => {
  const inicio = (paginaActual.value - 1) * productosPorPagina
  const fin = inicio + productosPorPagina
  return productosFiltrados.value.slice(inicio, fin)
})

const API_URL = 'https://productos-backend-wcpx.onrender.com/productos'

// Reiniciar a pagina 1 cuando se busca
watch(filtro, () => {
  paginaActual.value = 1
})

const paginaAnterior = () => {
  if (paginaActual.value > 1) {
    paginaActual.value--
  }
}

const paginaSiguiente = () => {
  if (paginaActual.value < totalPaginas.value) {
    paginaActual.value++
  }
}

const cargarProductos = async () => {
  try {
    const res = await axios.get(API_URL)
    productos.value = res.data
  } catch (error) {
    toast.error('Error al cargar productos')
  }
}

const agregarProducto = async () => {
  if (!nuevoProducto.value.nombre || nuevoProducto.value.precio <= 0) {
    toast.error('Nombre y precio valido son obligatorios')
    return
  }
  try {
    await axios.post(API_URL, nuevoProducto.value)
    nuevoProducto.value = { nombre: '', precio: 0 }
    await cargarProductos()
    toast.success('Producto agregado correctamente')
  } catch (error) {
    toast.error('Error al agregar producto')
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
    await cargarProductos()
    toast.success('Producto actualizado correctamente')
  } catch (error) {
    toast.error('Error al editar producto')
  }
}

const cancelarEdicion = () => {
  editando.value = null
}

const eliminarProducto = async (id) => {
  if (!confirm('Eliminar este producto?')) return
  try {
    await axios.delete(`${API_URL}/${id}`)
    await cargarProductos()
    toast.success('Producto eliminado correctamente')
  } catch (error) {
    toast.error('Error al eliminar producto')
  }
}

onMounted(cargarProductos)
</script>