<script setup>
import { reactive, computed } from 'vue'
import Alerta from './Alerta.vue'

const alerta = reactive({
  tipo: '',
  mensaje: ''
})

const emits = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente'])

const props = defineProps({
  id: {
    type: [String, null],
    required: true
  },
  nombre: {
    type: String,
    required: true
  },
  propietario: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  alta: {
    type: String,
    required: true
  },
  sintomas: {
    type: String,
    required: true
  },
  
})

const validar = () => {
  if(Object.values(props).includes('')){
    alerta.mensaje = 'Todos los campos son obligatorios'
    alerta.tipo = 'error'
    return;
  }


  emits('guardar-paciente')
  alerta.mensaje = 'Paciente almacenado correctamente'
  alerta.tipo = 'exito'

  setTimeout(() => {
    Object.assign(alerta, {
      mensaje: '',
      tipo: ''
    })
  }, 3000)
}

const editando = computed(() => {
  return props.id
})

</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento de Pacientes</h2>

    <p class="text-lg mt-5 text-center mb-10">
      Añadir Pacientes y
      <span class="text-indigo-600 font-bold"> Administralos</span>
    </p>

    <Alerta v-if="alerta.mensaje" :alerta="alerta" />
    <form @submit.prevent="validar" class="bg-white shadow-md rounded-lg py-10 px-5 mb-10">
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">Nombre Mascota</label>

        <input :value="nombre" @input="$emit('update:nombre', $event.target.value)" type="text" placeholder="Nombre de la mascota" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>

      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold">Nombre Propietario</label>

        <input :value="propietario" @input="$emit('update:propietario', $event.target.value)" id="propietario" type="text" placeholder="Nombre del propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">Correo Electrónico</label>

        <input :value="email" @input="$emit('update:email', $event.target.value)" id="email" type="email" placeholder="Email del propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">Alta</label>

        <input :value="alta" @input="$emit('update:alta', $event.target.value)" id="alta" type="date" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md">
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">Sintomas</label>

        <textarea :value="sintomas" @input="$emit('update:sintomas', $event.target.value)" id="sintomas" placeholder="Describe los sintomas de tu mascotas" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40" />
      </div>

      <input type="submit" class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors" 
        :value="[editando ? 'Guardar cambios' : 'Registrar pacientes']"
      >
    </form>
  </div>
</template>
