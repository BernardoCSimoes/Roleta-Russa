<template>
  <div class="fixed inset-0 z-50 bg-black/70 flex items-center justify-center">
    <div class="p-[1px] rounded-xl bg-gradient-to-b from-white to-gray-500">
      <div class="bg-slate-800 rounded-xl p-6 flex flex-row items-center gap-8 w-[500px]">

        <!-- lado esquerdo: ícone -->
        <img src="../img/bonezone-dancing.gif" alt="caveira dançando" class="w-20 h-20 flex-shrink-0" />

        <!-- lado direito: texto + botão -->
        <div class="flex flex-col gap-3 flex-1">
          <h2 class="text-red-500 text-2xl font-bold">Você perdeu!</h2>
          <p class="text-gray-300 text-sm">Quantidade de tiros: {{ props.quantTiros }}/{{ props.total }}</p>
          <p class="text-gray-400 text-sm">Mais sorte na próxima 🍀</p>
          <button
            class="bg-red-700 hover:bg-red-800 text-white font-semibold px-6 py-2 rounded-lg cursor-pointer transition-colors duration-200 w-full mt-1"
            @click="$emit('Reinicio')">
            Reiniciar
          </button>
        </div>

      </div>
    </div>
  </div>
</template>

<script setup lang="ts">

const props = defineProps(['quantTiros', 'total']);


</script>


<style scoped>
@media (orientation: landscape) and (max-width: 768px) {
  .card-inner {
    flex-direction: row;
    width: auto;
    align-items: center;
    gap: 1.5rem;
  }

  .card-text {
    align-items: flex-start;
  }

  .skull {
    order: 1;
    flex-shrink: 0;
  }

  .card-text {
    order: 0;
  }
}
</style>