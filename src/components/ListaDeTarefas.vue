<script setup>
    const props = defineProps(["filtrarTarefas", "estado"]);
    function removeItem(item) {
        const comfirm = confirm("Are you sure you want to remove?")
        if (comfirm){
            props.filtrarTarefas.pop(item);
            return localStorage.setItem('tarefas',JSON.stringify(props.filtrarTarefas))
        }
        return
    }
    const ChecarTarefa = (event, tarefa)=>{
        tarefa.finalizada = event.target.checked
        localStorage.setItem('tarefas',JSON.stringify(props.estado.tarefas))
    }
    const listaa = props.filtrarTarefas.length
</script>
<template>
    <div>
        <hr>
        <h3 id="mensagem" v-if="props.filtrarTarefas.length <= 0">Você nãotem tarefas aqui.</h3>
    </div>
    <ul class="list-group mt-4" v-for="tarefa in props.filtrarTarefas">
        <li
        class="list-group-item d-flex justify-content-between align-items-center"
        >
            <div>
                <input
                    :id="tarefa.titulo"
                    @change="event => ChecarTarefa(event, tarefa)"
                    :checked="tarefa.finalizada"
                    type="checkbox"
                />
                <label
                    :class="{ done: tarefa.finalizada }"
                    :for="tarefa.titulo"
                    class="ms-3"
                >
                    {{ tarefa.titulo }}
                </label>
            </div>
            <button type="button" @click="removeItem(tarefa)" class="btn btn-danger">
                Apagar
            </button>
        </li>

    </ul>
</template>

<style>
    #mensagem {
        display: flex;
        justify-content: center;
        margin: 5% auto;
    }
</style>