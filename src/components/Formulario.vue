<script setup>
    const props = defineProps(['estado', 'adicionarTarefa'])
</script>

<template>
    <form @submit.prevent="props.adicionarTarefa">
        <div class="row">
            <div class="col-md-7 col-lg">
                <input
                    :value="props.estado.inputValue"
                    @keyup="event => props.estado.inputValue = event.target.value"
                    type="text"
                    placeholder="Descrição da tarefa"
                    class="form-control"
                />
            </div>
            <div class="mt-2 mb-3 mt-md-0 mb-md-0 col-md-2">
                <button
                    v-if="props.estado.inputValue !== ''"
                    type="submit"
                    class="btn btn-primary"
                >
                    Cadastrar
                </button>
                <button v-else type="submit" class="btn btn-primary" disabled>
                    Cadastrar
                </button>
            </div>
            <div class="col-md-3">
                <select
                    @change="(event) => (props.estado.filtro = event.target.value)"
                    class="form-control"
                >
                    <option value="todas">Todas as tarefas</option>
                    <option value="pendentes">Pendentes</option>
                    <option value="finalizadas">Fonalizadas</option>
                </select>
            </div>
        </div>
    </form>
</template>
