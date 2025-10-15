<script setup lang="ts">
import { ref } from 'vue';
import EntradaTemperatura from './components/EntradaTemperatura.vue';

const celsius = ref('');
const fahrenheit = ref('');
const kelvin = ref('');
const erroKelvin = ref('');

// Funções de conversão
const paraCelsius = (valor: number, de: string): number => {
  if (de === 'fahrenheit') {
    return (valor - 32) * 5 / 9;
  } else if (de === 'kelvin') {
    return valor - 273.15;
  }
  return valor;
};

const paraFahrenheit = (valor: number, de: string): number => {
  if (de === 'celsius') {
    return (valor * 9 / 5) + 32;
  } else if (de === 'kelvin') {
    return (valor - 273.15) * 9 / 5 + 32;
  }
  return valor;
};

const paraKelvin = (valor: number, de: string): number => {
  if (de === 'celsius') {
    return valor + 273.15;
  } else if (de === 'fahrenheit') {
    return (valor - 32) * 5 / 9 + 273.15;
  }
  return valor;
};

const aoMudarTemperatura = (valor: string, unidade: string) => {
  erroKelvin.value = '';

  if (valor === '' || valor === '-') {
    celsius.value = '';
    fahrenheit.value = '';
    kelvin.value = '';
    return;
  }

  const valorNum = parseFloat(valor);

  if (isNaN(valorNum)) {
    return;
  }

  // Validação: Kelvin não pode ser menor que 0
  if (unidade === 'kelvin' && valorNum < 0) {
    erroKelvin.value = 'Kelvin não pode ser menor que 0';
    kelvin.value = valor;
    celsius.value = '';
    fahrenheit.value = '';
    return;
  }

  // Validação: Se converter para Kelvin e der negativo
  const kelvinCalculado = unidade !== 'kelvin' ? paraKelvin(valorNum, unidade) : valorNum;
  if (kelvinCalculado < 0) {
    erroKelvin.value = 'Temperatura inválida (Kelvin < 0)';
  }

  if (unidade === 'celsius') {
    celsius.value = valor;
    fahrenheit.value = paraFahrenheit(valorNum, 'celsius').toFixed(2);
    kelvin.value = paraKelvin(valorNum, 'celsius').toFixed(2);
  } else if (unidade === 'fahrenheit') {
    fahrenheit.value = valor;
    celsius.value = paraCelsius(valorNum, 'fahrenheit').toFixed(2);
    kelvin.value = paraKelvin(valorNum, 'fahrenheit').toFixed(2);
  } else if (unidade === 'kelvin') {
    kelvin.value = valor;
    celsius.value = paraCelsius(valorNum, 'kelvin').toFixed(2);
    fahrenheit.value = paraFahrenheit(valorNum, 'kelvin').toFixed(2);
  }
};
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-400 via-purple-500 to-green-400 flex items-center justify-center p-4">
    <div class="absolute inset-0 bg-white opacity-10 backdrop-blur-sm"></div>
    
    <div class="relative bg-white rounded-3xl shadow-2xl p-10 w-full max-w-lg border-4 border-white">
      <!-- Cabeçalho -->
      <div class="text-center mb-8">
        <div class="inline-block p-4 bg-gradient-to-r from-green-400 to-blue-500 rounded-full mb-4">
          <span class="text-5xl">🌡️</span>
        </div>
        <h1 class="text-4xl font-bold bg-gradient-to-r from-blue-600 via-purple-600 to-green-600 bg-clip-text text-transparent">
          Conversor de Temperatura
        </h1>
        <p class="mt-2 text-gray-600 font-medium">
          Converta entre diferentes escalas termométricas
        </p>
      </div>
      
      <!-- Campos de entrada -->
      <div class="space-y-2">
        <div class="bg-gradient-to-r from-green-50 to-green-100 p-5 rounded-2xl border-2 border-green-200">
          <EntradaTemperatura
            label="🌿 Celsius (°C)"
            :valor="celsius"
            unidade="celsius"
            @ao-mudar="aoMudarTemperatura"
            erro=""
            cor="text-green-600"
          />
        </div>

        <div class="bg-gradient-to-r from-blue-50 to-blue-100 p-5 rounded-2xl border-2 border-blue-200">
          <EntradaTemperatura
            label="💧 Fahrenheit (°F)"
            :valor="fahrenheit"
            unidade="fahrenheit"
            @ao-mudar="aoMudarTemperatura"
            erro=""
            cor="text-blue-600"
          />
        </div>

        <div class="bg-gradient-to-r from-purple-50 to-purple-100 p-5 rounded-2xl border-2 border-purple-200">
          <EntradaTemperatura
            label="⚗️ Kelvin (K)"
            :valor="kelvin"
            unidade="kelvin"
            @ao-mudar="aoMudarTemperatura"
            :erro="erroKelvin"
            cor="text-purple-600"
          />
        </div>
      </div>

      <!-- Dica -->
      <div class="mt-8 p-5 bg-gradient-to-r from-blue-100 via-purple-100 to-green-100 rounded-2xl border-2 border-purple-200">
        <div class="flex items-center justify-center gap-3">
          <span class="text-3xl">💡</span>
          <p class="text-sm text-gray-700 font-semibold">
            Digite um valor em qualquer campo para converter automaticamente!
          </p>
        </div>
      </div>

      <!-- Decoração -->
      <div class="mt-6 flex justify-center gap-3">
        <div class="w-3 h-3 bg-green-500 rounded-full animate-pulse"></div>
        <div class="w-3 h-3 bg-blue-500 rounded-full animate-pulse" style="animation-delay: 0.2s"></div>
        <div class="w-3 h-3 bg-purple-500 rounded-full animate-pulse" style="animation-delay: 0.4s"></div>
      </div>
    </div>
  </div>
</template>
