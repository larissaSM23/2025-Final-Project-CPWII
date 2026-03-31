<template>
      <h3>Adicionar nova transação</h3>
      <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
          <label for="text">Descrição</label>
          <input type="text" id="text" v-model="text" placeholder="Digite a descrição" />
        </div>
        <div class="form-control">
          <label for="valor">Valor <br />(negativo - despesa, positivo - receita)</label>
          <input type="text" id="valor" v-model="valor" placeholder="Digite o valor" />
        </div>
        <button class="btn">Adicionar transação</button>
      </form>
</template>

<script setup>
  import { ref } from 'vue';
  import { useToast } from 'vue-toastification';

  const text = ref('');
  const valor = ref('');

  const emit = defineEmits(['transacaoAdicionada']);
  const toast = useToast();

  const onSubmit = () => {
    if(!text.value || !valor.value) {
      toast.error('Ambos os campos devem ser preenchidos');
      return;
    }

    const novaTransacao = {
      text: text.value,
      valor: parseFloat(valor.value),
    };

    emit('transacaoAdicionada', novaTransacao);

    text.value = '';
    valor.value = '';
  };
</script>