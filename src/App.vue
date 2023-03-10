<script setup>
import {ref, computed} from "vue"
import {productos} from "./datos.js"

const counter = ref(0)
const total = productos.length;


const next = () => {
  counter.value++;

  if(counter.value >= total) {
    counter.value = 0
  }
}

const king = computed(() => {
  const elNombre = productos[counter.value].nombre.toLowerCase();
  return elNombre.substring(0,1).toUpperCase() + elNombre.substring(1)
})

const imagen = computed(() => {
  const kingName = productos[counter.value].nombre.toLowerCase()
  return `https://www.html6.es/img/rey_${kingName}.png`
})

const newPrice = computed(() => {
  const discount = 10
  return Number(productos[counter.value].precio - (productos[counter.value].precio * discount)/100).toFixed(2)
})
</script>


<template>
  <div class="card">
    <h2>Cena {{ counter + 1 }} con el rey godo {{ king }}</h2>
    <h3 class="precio">Precio: {{ productos[counter].precio }}€</h3>
    
    <div class="weekend days" v-if="productos[counter].finDeSemana">(Sólo fines de semana)</div>
    <div v-else class="all-days days">(De lunes a domingo)</div>

    <div v-if="productos[counter].precio < 100" class="offer">
      <div>Ahora un 10% dto:
        {{ newPrice }}€
        <img src="oferta.jpg" alt="Rey godo en descuento">
      </div>
    </div>

    <img 
      :src="imagen" 
      :alt="productos[counter].nombre.toLowerCase"
    >
    <button @:click="next">Siguiente ({{ counter + 1 }} / {{ total }})</button>
  </div>
</template>


<style scoped>
  h2 {
    margin: 0;
  }
  .all-days {
    background-color: green;
  }
  .weekend {
    background-color: red;
  }
  .days {
    padding: 4px 16px;
    font-size: 14px;
    width: fit-content;
    color: white;
    border-radius: 4px;
    margin: 5px 0 10px;
  }

  .offer {
    margin-block-end: 32px;
  }
  .offer > div {
    display: flex;
    align-items: center;
  }
  .offer img {
    width: 56px;
    margin-inline-start: 8px;
  }

  button {
    margin-block-start: 32px;
  }
</style>
