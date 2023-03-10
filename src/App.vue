<script setup>
import {ref, computed} from "vue"
import {productos} from "./datos.js"

const contador = ref(0)
const total = productos.length;


const siguiente = () => {
  contador.value++;

  if(contador.value >= total) {
    contador.value = 0
  }
}

const rey = computed(() => {
  const elNombre = productos[contador.value].nombre.toLowerCase();
  return elNombre.substring(0,1).toUpperCase() + elNombre.substring(1)
})

const imagen = computed(() => {
  const nombreDelRey = productos[contador.value].nombre.toLowerCase()
  return `https://www.html6.es/img/rey_${nombreDelRey}.png`
})

const nuevoPrecio = computed(() => {
  const descuento = 10
  return Number(productos[contador.value].precio - (productos[contador.value].precio * descuento)/100).toFixed(2)
})
</script>


<template>
  <div class="card">
    <h2>Cena {{ contador + 1 }} con el rey godo {{ rey }}</h2>
    <h3 class="precio">Precio: {{ productos[contador].precio }}€</h3>
    
    <div class="weekend dias" v-if="productos[contador].finDeSemana">(Sólo fines de semana)</div>
    <div v-else class="todos-los-dias dias">(De lunes a domingo)</div>

    <div v-if="productos[contador].precio < 100" class="oferta">
      <div>Ahora un 10% dto:
        {{ nuevoPrecio }}€
        <img src="oferta.jpg" alt="rey godo en descuento">
      </div>
    </div>

    <img :src="imagen" :alt="productos[contador].nombre.toLowerCase">
    <button @:click="siguiente">Siguiente ({{ contador + 1 }} / {{ total }})</button>
  </div>
</template>


<style scoped>
  h2 {
    margin: 0;
  }
  .todos-los-dias {
    background-color: green;
  }
  .weekend {
    background-color: red;
  }
  .dias {
    padding: 4px 16px;
    font-size: 14px;
    width: fit-content;
    color: white;
    border-radius: 4px;
    margin: 5px 0 10px;
  }

  .oferta {
    margin-block-end: 32px;
  }
  .oferta > div {
    display: flex;
    align-items: center;
  }
  .oferta img {
    width: 56px;
    margin-inline-start: 8px;
  }

  button {
    margin-block-start: 32px;
  }
</style>
