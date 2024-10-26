<script setup>
import { reactive, ref } from 'vue';

const titulo = ref("")
const ano = ref("")
const genero = ref("")

const filmes = reactive([])


function criarFilme() {

    const tituloVazio = titulo.value.trim() === ""
    const anoVazio = ano.value === ""
    const generoVazio = genero.value.trim() === ""

    if ( tituloVazio || anoVazio || generoVazio) {
        return alert("Informações faltantes para criação do filme. Preencha os campos corretamente")
    }

    const anoMenorQuePrimeiroFilmeCriado = ano.value < 1895
    const anoMaiorQueAnoAtual = ano.value > new Date().getFullYear()

    if(anoMenorQuePrimeiroFilmeCriado || anoMaiorQueAnoAtual) {
        return alert("Ano informado inválido")
    }


    filmes.push(
        {
            titulo: titulo.value,
            ano: ano.value, 
            genero: genero.value,
        }
    )

    titulo.value = ""
    ano.value = ""
    genero.value = ""

}

function excluirFilme(filme) {
    filmes.forEach((itemFilme) => {
        if(itemFilme === filme) {
            const index = filmes.indexOf(itemFilme)
            if(index > -1) {
                filmes.splice(index, 1)
            }
        }
    })
}

</script>

<template>
    <h1>Incuir filme</h1>

    <div class="movieCreationContainer">
        <label>
            Título
            <input v-model="titulo" type="text">
        </label>
        <label>
            Ano
            <input v-model="ano" type="number">
        </label>
        <label>
            Gênero
            <input v-model="genero" type="text">
        </label>
    
        <button @click="criarFilme">Criar</button>
    </div>

    <hr class="division">

    <div>
        <h2>Lista de filmes</h2>
        <template v-if="filmes.length === 0">
            <div>Não existem filmes cadastrados</div>
        </template>
        <v-else>
            <template v-for="filme in filmes">
                <div class="movieBox">
                    <div>Título: {{ filme.titulo }}</div>
                    <div>Ano: {{ filme.ano }}</div>
                    <div>Gênero: {{ filme.genero }}</div>
                    <button @click="excluirFilme(filme)">Excluir</button>
                </div>
            </template>
        </v-else>
    </div>
</template>

