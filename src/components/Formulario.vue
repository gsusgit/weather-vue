<script setup>

  import { reactive, ref } from 'vue'
  import Alerta from './Alerta.vue'

  const emits = defineEmits(['obtener-clima'])

  const paises = ref([
    { codigo: 'US', nombre: 'Estados Unidos' },
    { codigo: 'MX', nombre: 'México' },
    { codigo: 'AR', nombre: 'Argentina' },
    { codigo: 'CO', nombre: 'Colombia' },
    { codigo: 'CR', nombre: 'Costa Rica' },
    { codigo: 'ES', nombre: 'España' },
    { codigo: 'PE', nombre: 'Perú' }
  ])

  const formulario = reactive({
    ciudad: 'Córdoba',
    pais: 'ES'
  })

  const error = ref('')

  const consultarClima = () => {
    if (Object.values(formulario).includes('')) {
      error.value = 'Rellena los campos'
      setTimeout(() => {
        error.value = ''
      }, 1500)
      return
    }
    emits('obtener-clima', formulario)
  }

</script>

<template>
  <form
      @submit.prevent="consultarClima"
      class="formulario">
    <div class="campo">
      <label for="ciudad">Ciudad</label>
      <input
          type="text"
          id="ciudad"
          placeholder="Escribir ciudad"
          v-model="formulario.ciudad">
    </div>
    <div class="campo">
      <label for="pais">Países</label>
      <select
          v-model="formulario.pais"
          id="pais">
        <option value="">Seleccionar país</option>
        <option
            v-for="pais in paises"
            :value="pais.codigo">
          {{ pais.nombre }}
        </option>
      </select>
    </div>
    <input
        type="submit"
        value="Consultar clima">
    <Alerta
        v-if="error !== ''">
      {{ error }}
    </Alerta>
  </form>
</template>
