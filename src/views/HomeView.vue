<template>
  <div class="container">
    <h1>Productos</h1>
    
    <!-- Formulario para agregar producto -->
    <div class="form">
      <input v-model="nuevoProducto.nombre" placeholder="Nombre" />
      <input v-model.number="nuevoProducto.precio" placeholder="Precio" type="number" />
      <button @click="agregarProducto">Agregar</button>
    </div>

    <!-- Tabla de productos -->
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombre</th>
          <th>Precio</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="producto in productos" :key="producto.id">
          <td>{{ producto.id }}</td>
          <td>
            <span v-if="editando !== producto.id">{{ producto.nombre }}</span>
            <input v-else v-model="editProducto.nombre" />
          </td>
          <td>
            <span v-if="editando !== producto.id">${{ producto.precio }}</span>
            <input v-else v-model.number="editProducto.precio" type="number" />
          </td>
          <td>
            <button v-if="editando !== producto.id" @click="editarProducto(producto)">Editar</button>
            <button v-else @click="guardarEdicion(producto.id)">Guardar</button>
            <button @click="eliminarProducto(producto.id)">Eliminar</button>
            <button v-if="editando === producto.id" @click="cancelarEdicion">Cancelar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const productos = ref([])
const nuevoProducto = ref({ nombre: '', precio: 0 })
const editando = ref(null)
const editProducto = ref({ nombre: '', precio: 0 })

const cargarProductos = async () => {
  try {
    const response = await axios.get('https://productos-backend-wcpx.onrender.com/productos')
    productos.value = response.data
  } catch (error) {
    console.error('Error al cargar productos:', error)
  }
}

const agregarProducto = async () => {
  try {
    await axios.post('https://productos-backend-wcpx.onrender.com/productos', nuevoProducto.value)
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
    await axios.put(`https://productos-backend-wcpx.onrender.com/productos/${id}`, editProducto.value)
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
  try {
    await axios.delete(`https://productos-backend-wcpx.onrender.com/productos/${id}`)
    cargarProductos()
  } catch (error) {
    console.error('Error al eliminar producto:', error)
  }
}

onMounted(() => {
  cargarProductos()
})
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #42b883;
}

.form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.form input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  margin: 0 4px;
}

button:hover {
  background-color: #359268;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}
</style>