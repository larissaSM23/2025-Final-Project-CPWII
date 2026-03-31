<template>
    <Header></Header>
    <div class="container">
        <Balanco :total="total"/>
        <ReceitaDespesa :receita="+receitas" :despesa="+despesas"/>
        <div class="filtro-transacoes">
            <label for="filtro">Filtrar por:</label>
            <select id="filtro" v-model="filtroTransacao">
                <option value="todas">Todas</option>
                <option value="receitas">Receitas</option>
                <option value="despesas">Despesas</option>
            </select>
        </div>
        <ListaTransacao :transacoes="transacoesFiltradas" @removerTransacao="remTransacao"/>
        <AdicionaTransacao @transacaoAdicionada="transacaoAdd"/>
    </div>
</template>

<script setup>
    import Header from './components/Header.vue';
    import Balanco from './components/Balanco.vue';
    import ReceitaDespesa from './components/ReceitaDespesa.vue';
    import ListaTransacao from './components/ListaTransacao.vue';
    import AdicionaTransacao from './components/AdicionaTransacao.vue';

    import { useToast } from 'vue-toastification';
    import { ref, computed, onMounted} from 'vue';

    const toast = useToast();
    const transacoes = ref ([]);

    onMounted(() => {
        const transacoesSalvas = JSON.parse(localStorage.getItem('transacoes'));

        if(transacoesSalvas) {
            transacoes.value = transacoesSalvas;

        }
    })
    
    //pega o total
    const total = computed(() => {
        return transacoes.value.reduce((acc, transacao) => {
            return acc + transacao.valor;
        }, 0);
    });

    //pega despesa
    const despesas = computed(() => {
        return transacoes.value.filter((transacao) => transacao.valor < 0).reduce((acc, transacao) => {
            return acc + transacao.valor;
        }, 0).toFixed(2);
    });

    //pega receita
    const receitas = computed(() => {
        return transacoes.value.filter((transacao) => transacao.valor > 0).reduce((acc, transacao) => {
            return acc + transacao.valor;
        }, 0).toFixed(2);
    });

    //adicionar transação
    const transacaoAdd = (novaTransacao) => {
        transacoes.value.push({
            id: geraIdUnico(),
            descricao: novaTransacao.text,
            valor: novaTransacao.valor,
        });

        salvarLocalStorage();

        toast.success('Transação adicionada!');
    };

    //gerar id unico
    const geraIdUnico = () => {
        return Math.floor(Math.random() * 1000000);
    }

    //remover transacao
    const remTransacao = (id) => {
        transacoes.value = transacoes.value.filter((transacao) => transacao.id !== id);

        salvarLocalStorage();

        toast.success('Transação removida!');
    };

    //salvar no local storage
    const salvarLocalStorage = () => {
        localStorage.setItem('transacoes', JSON.stringify(transacoes.value));
    };

    //filtro de transações
    const filtroTransacao = ref("todas");

    //lista filtrada
    const transacoesFiltradas = computed(() => {
        if(filtroTransacao.value == "todas") {
            return transacoes.value;
        } else if(filtroTransacao.value == "receitas") {
            return transacoes.value.filter((transacao) => transacao.valor > 0);
        } else if(filtroTransacao.value == "despesas") {
            return transacoes.value.filter((transacao) => transacao.valor < 0);
        }
    })
</script>