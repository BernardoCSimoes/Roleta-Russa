<template>
    <div
        class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 p-[1px] rounded-xl bg-gradient-to-b from-white to-gray-500">
        <div class="bg-zinc-800 rounded-xl p-6 flex flex-col items-center gap-4 w-72">

            <img src="" alt="caveira dançando" class="w-24 h-24">

            <h2 class="text-white text-2xl font-bold">Você perdeu!</h2>

            <p class="text-gray-300 text-sm">Quantidade de tiros: {{ props.quantTiros }}/{{ props.total }}</p>

            <p class="text-gray-400 text-sm">Mais sorte na próxima 🍀</p>

            <button
                class="bg-slate-600 hover:bg-slate-500 text-white font-semibold px-6 py-2 rounded-lg cursor-pointer transition-colors duration-200 w-full"
                @click="$emit('Reinicio')">
                Reiniciar
            </button>

        </div>
    </div>
</template>

<script setup>

const props = defineProps(['quantTiros', 'total']);


</script>


<style scoped></style>