<script setup>
    import { reactive } from "vue";
    import Cabecalho from './components/Cabecalho.vue';
    import Formulario from './components/Formulario.vue';
    import ListaDeTarefas from './components/ListaDeTarefas.vue';
    const local = localStorage.getItem('tarefas');
    console.log(local)
    if(local == null){
        localStorage.setItem('tarefas', JSON.stringify([]));
    }

    const estado = reactive({
        filtro: "todas",
        tarefas: JSON.parse( localStorage.getItem('tarefas')),
        inputValue: "",
    });

    const checkTarefasPendentes = () => {
        return estado.tarefas.filter((tarefa) => !tarefa.finalizada);
    };
    const checkTarefasFinalizadas = () => {
        return estado.tarefas.filter((tarefa) => tarefa.finalizada);
    };

    const filtrarTarefas = () => {
        const { filtro } = estado;

        switch (filtro) {
            case "pendentes":
                return checkTarefasPendentes();
            case "finalizadas":
                return checkTarefasFinalizadas();
            default:
                return estado.tarefas;
        }
    };

    function adicionarTarefa() {
        const adicionaTarefa ={
            titulo: estado.inputValue,
            finalizada: false,
        };
        estado.tarefas.push(adicionaTarefa);
        const  updateTarefas = JSON.parse( localStorage.getItem('tarefas'))
        updateTarefas.push(adicionaTarefa);
        localStorage.setItem('tarefas', JSON.stringify(updateTarefas));
        estado.inputValue = "";
    }
</script>

<template>
    <div class="container">
       <Cabecalho :tarefas-pendentes="checkTarefasPendentes().length" />
       <Formulario :estado="estado" :adicionar-tarefa="adicionarTarefa"/>
       <ListaDeTarefas :filtrar-tarefas="filtrarTarefas()" :estado="estado" />
    </div>
</template>

<style>
.done {
    text-decoration: line-through;
}
</style>
