<script setup lang="ts">
defineProps<{
  label: string;
  valor: string;
  unidade: string;
  erro: string;
  cor: string;
}>();

const emit = defineEmits<{
  aoMudar: [valor: string, unidade: string];
}>();
</script>

<template>
  <div class="mb-6">
    <label :class="`block mb-2 font-bold text-lg ${cor}`">
      {{ label }}
    </label>
    <input
      type="number"
      :value="valor"
      @input="emit('aoMudar', ($event.target as HTMLInputElement).value, unidade)"
      :placeholder="`Digite em ${label}`"
      :class="[
        'w-full px-4 py-3 border-2 rounded-xl focus:outline-none transition-all duration-300 text-lg',
        erro 
          ? 'border-red-500 bg-red-50 focus:ring-4 focus:ring-red-200'
          : 'border-gray-200 bg-white focus:ring-4'
      ]"
    />
    <div v-if="erro" class="mt-2 p-3 bg-red-100 border-l-4 border-red-500 rounded">
      <p class="text-sm text-red-700 font-semibold">⚠️ {{ erro }}</p>
    </div>
  </div>
</template>
