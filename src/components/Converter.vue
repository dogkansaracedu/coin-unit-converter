<template>
    <div class="converter">
      <div class="input-group">
        <label for="sol">SOL</label>
        <input id="sol" v-model="sol" type="number" @input="updateUnits('sol')" />
      </div>
      <div class="input-group">
        <label for="lamports">Lamports</label>
        <input id="lamports" v-model="lamports" type="number" @input="updateUnits('lamports')" />
      </div>
      <div class="input-group">
        <label for="microLamports">Micro-Lamports</label>
        <input id="microLamports" v-model="microLamports" type="number" @input="updateUnits('microLamports')" />
      </div>
    </div>
  </template>
  
  <script lang="ts" setup>
  import BigNumber from 'bignumber.js';
import { ref } from 'vue';
  
  // Conversion rates
  const LAMPORTS_PER_SOL = new BigNumber(1e9);
  const MICRO_LAMPORTS_PER_SOL = new BigNumber(1e15);
  
  // Reactive inputs
  const sol = ref<string | null>(null);
  const lamports = ref<string | null>(null);
  const microLamports = ref<string | null>(null);
  
  function updateUnits(inputType: string) {
    try {
      if (inputType === 'sol') {
        const solValue = sol.value ? new BigNumber(sol.value) : null;
        lamports.value = solValue ? solValue.multipliedBy(LAMPORTS_PER_SOL).toFixed() : null;
        microLamports.value = solValue ? solValue.multipliedBy(MICRO_LAMPORTS_PER_SOL).toFixed() : null;
      } else if (inputType === 'lamports') {
        const lamportsValue = lamports.value ? new BigNumber(lamports.value) : null;
        sol.value = lamportsValue ? lamportsValue.dividedBy(LAMPORTS_PER_SOL).toFixed(9) : null;
        microLamports.value = lamportsValue ? lamportsValue.multipliedBy(1e6).toFixed() : null;
      } else if (inputType === 'microLamports') {
        const microLamportsValue = microLamports.value ? new BigNumber(microLamports.value) : null;
        sol.value = microLamportsValue ? microLamportsValue.dividedBy(MICRO_LAMPORTS_PER_SOL).toFixed(15) : null;
        lamports.value = microLamportsValue ? microLamportsValue.dividedBy(1e6).toFixed() : null;
      }
    } catch (error) {
      console.error('Error during conversion:', error);
    }
  }
  </script>
  
  <style scoped>
  .converter {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  .input-group {
    display: flex;
    flex-direction: column;
  }
  label {
    font-weight: bold;
  }
  input {
    padding: 0.5rem;
    font-size: 1rem;
  }
  </style>
  