<script setup lang="ts">

import { ref } from 'vue';
import BangPopUp from './components/BangPopUp.vue';

const tamborTiros = [0, 0, 0, 0, 0, 1];
let perdeu = ref(false);
let percorreTiro = ref(0);
const animando = ref(false);

function Shuffle(lista: number[]) {

  for (let i = tamborTiros.length - 1; i > 1; i--) {

    let j = Math.floor(Math.random() * (i + 1));

    [lista[i], lista[j]] = [lista[j]!, lista[i]!];

  }
}

function Atirar(lista: number[]) {

  if (lista[percorreTiro.value] == 1) {
    animando.value = true;
    setTimeout(() => animando.value = false, 200);
    perdeu.value = true;
  }
  else {
    percorreTiro.value++;
  }

}

function Reiniciar(lista: number[]) {
  percorreTiro.value = 0;
  perdeu.value = false;
  Shuffle(lista);
}

</script>

<template>
  <div class="bg-zinc-900 text-gray-300 w-screen h-screen
  flex flex-col justify-center items-center">
    <!-- -->
    <BangPopUp v-if="perdeu" :quantTiros="percorreTiro" :total="tamborTiros.length"
      @Reinicio="Reiniciar(tamborTiros)" />
    <p class="text-4xl font-bold mb-20">Roleta Russa</p>


    <img class="w-[300px] scale-x-[-1]" src="./img/pistola.png" alt="Imagem aqui" >


    <div class="w-screen mt-20 h-[60px] flex items-center justify-center">
      <div class="w-2/5 flex flex-row justify-between items-center">

        <button
          class="cursor-pointer rounded-lg bg-gray-600 px-4 py-2 hover:bg-gray-700 transition-color duration-300 font-bold"
          @click="Reiniciar(tamborTiros)">Recarregar</button>
        <p>Número de Balas {{ percorreTiro }} / {{ tamborTiros.length }}</p>
        <button
          class="cursor-pointer rounded-lg bg-gray-600 px-4 py-2 hover:bg-gray-700 transition-color duration-300 font-bold"
          @click="Atirar(tamborTiros)">Atirar</button>

      </div>
    </div>

  </div>


</template>

<style scoped></style>
