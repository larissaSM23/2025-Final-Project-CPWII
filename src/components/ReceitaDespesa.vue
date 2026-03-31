<template>
    <div class="inc-exp-container">
        <div>
          <h4>Receita</h4>
          <p id="money-plus" class="money plus">+R${{ receita }}</p>
        </div>
        <div>
          <h4>Despesa</h4>
          <p id="money-minus" class="money minus">R${{ despesa }}</p>
        </div>
      </div>
</template>

<script setup>
  import {defineProps} from 'vue';

  const props = defineProps({
    receita: {
      type: Number,
      required: true,
    },
    despesa: {
      type: Number,
      required: true,
    },
  });
</script>