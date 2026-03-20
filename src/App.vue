<script setup lang="ts">

import { ref } from 'vue';
import BangPopUp from './components/BangPopUp.vue';

const tamborTiros = [0, 0, 0, 0, 0, 1];
let perdeu = ref(false);
let percorreTiro = ref(0);

function Shuffle(lista: number[]) {

  for (let i = tamborTiros.length - 1; i > 1; i--) {

    let j = Math.floor(Math.random() * (i + 1));

    [lista[i], lista[j]] = [lista[j]!, lista[i]!];

  }
}

function Atirar(lista: number[]) {

  if (lista[percorreTiro.value] == 1)
    perdeu.value = true;
  else {
    percorreTiro.value++;
  }

}

function Reiniciar(lista: number[]) {
  percorreTiro.value = 0;
  Shuffle(lista);
}

</script>

<template>
  <div class="bg-zinc-900 text-gray-300 w-screen h-screen
  flex flex-col justify-center items-center">
    <!-- -->
    <BangPopUp v-if="perdeu" />
    <p class="text-4xl font-bold mb-20">Roleta Russa</p>


    <img src="" alt="Imagem aqui">


    <div class="w-screen mt-20 h-[60px] flex items-center justify-center">
      <div class="w-3/5 flex flex-row justify-between">
        <button class="" @click="Atirar(tamborTiros)">Atirar</button>
        <p>Número de Balas {{ percorreTiro }} / {{ tamborTiros.length }}</p>
        <button class="" @click="Reiniciar(tamborTiros)">Recarregar</button>
      </div>
    </div>

  </div>


</template>

<style scoped></style>
