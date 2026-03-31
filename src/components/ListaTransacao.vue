<template>
    <h3>Histórico</h3>
      <ul id="list" class="list">
        <li v-for="transacao in transacoes" v-bind:key="transacao.id" :class="transacao.valor < 0 ? 'minus' : 'plus'">
          {{transacao.descricao}} <span>R${{transacao.valor}}</span><button @click="removerTransacao(transacao.id)" class="delete-btn">x</button>
        </li>
      </ul>
</template>

<script setup>
  import { defineProps } from 'vue';

  const emit = defineEmits(['removerTransacao']);

  const props = defineProps({
    transacoes: {
      type: Array,
      reqired: true,
    },
  });

  const removerTransacao = (id) => {
    emit('removerTransacao', id);
  }
</script>