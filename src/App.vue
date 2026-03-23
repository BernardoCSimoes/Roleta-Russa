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

function playGunshot() {
  const ctx = new AudioContext();

  // Barulho branco (a "explosão")
  const bufferSize = Math.floor(ctx.sampleRate * 0.18);
  const buffer = ctx.createBuffer(1, bufferSize, ctx.sampleRate);
  const data = buffer.getChannelData(0);
  for (let i = 0; i < bufferSize; i++) data[i] = Math.random() * 2 - 1;

  const noise = ctx.createBufferSource();
  noise.buffer = buffer;

  const noiseFilter = ctx.createBiquadFilter();
  noiseFilter.type = 'bandpass';
  noiseFilter.frequency.value = 700;

  const noiseGain = ctx.createGain();
  noiseGain.gain.setValueAtTime(1.2, ctx.currentTime);
  noiseGain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.18);

  noise.connect(noiseFilter);
  noiseFilter.connect(noiseGain);
  noiseGain.connect(ctx.destination);
  noise.start();

  // Grave de impacto
  const osc = ctx.createOscillator();
  osc.frequency.setValueAtTime(180, ctx.currentTime);
  osc.frequency.exponentialRampToValueAtTime(40, ctx.currentTime + 0.12);

  const oscGain = ctx.createGain();
  oscGain.gain.setValueAtTime(1.5, ctx.currentTime);
  oscGain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.12);

  osc.connect(oscGain);
  oscGain.connect(ctx.destination);
  osc.start();
  osc.stop(ctx.currentTime + 0.12);
}

function playClick() {
  const ctx = new AudioContext();

  // Clique seco do martelo
  const bufferSize = Math.floor(ctx.sampleRate * 0.04);
  const buffer = ctx.createBuffer(1, bufferSize, ctx.sampleRate);
  const data = buffer.getChannelData(0);
  for (let i = 0; i < bufferSize; i++) data[i] = Math.random() * 2 - 1;

  const noise = ctx.createBufferSource();
  noise.buffer = buffer;

  const filter = ctx.createBiquadFilter();
  filter.type = 'highpass';
  filter.frequency.value = 2000;

  const gain = ctx.createGain();
  gain.gain.setValueAtTime(0.8, ctx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime + 0.04);

  noise.connect(filter);
  filter.connect(gain);
  gain.connect(ctx.destination);
  noise.start();
}

function Atirar(lista: number[]) {

  if (lista[percorreTiro.value] == 1) {
    animando.value = true;
    playGunshot();
    setTimeout(() => {
      animando.value = false;
      perdeu.value = true;
    }, 600);
  }
  else {
    playClick();
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
  <div class="bg-zinc-900 text-gray-300 w-screen h-screen flex flex-row items-center justify-between px-8 gap-6">
    <!-- -->
    <BangPopUp v-if="perdeu" :quantTiros="percorreTiro" :total="tamborTiros.length"
      @Reinicio="Reiniciar(tamborTiros)" />

    <div class="flex flex-col items-center gap-4">
      <p class="text-3xl font-bold">Roleta Russa</p>
      <!-- bolinhas representando as câmaras -->
      <div class="flex gap-2">
        <div v-for="(_, i) in tamborTiros" :key="i"
          :class="['w-4 h-4 rounded-full border-2', i < percorreTiro ? 'bg-red-500 border-red-600' : 'bg-zinc-600 border-zinc-700']">
        </div>
      </div>
      <p class="text-zinc-500 text-sm">{{ percorreTiro }} / {{ tamborTiros.length }} tiros</p>
    </div>

    <!-- coluna central: pistola -->
    <div class="flex-1 flex justify-center">
      <img class="w-[280px] scale-x-[-1]" :class="{ 'animate-bounce': animando }" src="./img/pistola.png"
        alt="pistola" />
    </div>

    <!-- coluna direita: botões -->
    <div class="flex flex-col gap-4 items-center">
      <button class="w-32 cursor-pointer rounded-lg bg-zinc-600 px-4 py-3 hover:bg-zinc-700 transition-colors font-bold"
        @click="Reiniciar(tamborTiros)">Recarregar</button>
      <button class="w-32 cursor-pointer rounded-lg bg-red-700 px-4 py-3 hover:bg-red-800 transition-colors font-bold"
        @click="Atirar(tamborTiros)">Atirar</button>
    </div>

  </div>


</template>

<style scoped>
@media (orientation: landscape) and (max-width: 768px) {
  .title {
    font-size: 1.5rem;
    margin-bottom: 0.75rem;
  }

  .gun {
    width: 150px;
  }

  .controls {
    margin-top: 0.75rem;
  }

  .controls>div {
    width: 80%;
  }
}

@keyframes coice {
  0% {
    transform: rotate(0deg) translateY(0);
  }

  30% {
    transform: rotate(-20deg) translateY(-18px);
  }

  100% {
    transform: rotate(0deg) translateY(0);
  }
}

.coice {
  animation: coice 0.35s ease-out forwards;
}
</style>
