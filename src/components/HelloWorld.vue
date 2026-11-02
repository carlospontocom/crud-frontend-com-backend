<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const contatos = ref([]);
const novoNome = ref('');
const novoEmail = ref('');

const url = 'https://6a8529b09c451dc67a6351f4.mockapi.io/usuarios';

onMounted(async () => {
  try {
    const { data } = await axios.get(url);
    contatos.value = data;
  } catch (error) {
    console.error('Erro ao buscar usuários:', error);
  }
});

const add = async () => {
  if (!novoNome.value.trim() || !novoEmail.value.trim()) return;

  try {
    const { data } = await axios.post(url, {
      nome: novoNome.value,
      email: novoEmail.value,
    });

    contatos.value.push(data);

    novoNome.value = '';
    novoEmail.value = '';
  } catch (error) {
    console.error('Erro ao adicionar usuário:', error);
  }
};

// 3. Remover
const remove = async (id) => {
  try {
    await axios.delete(`${url}/${id}`);
    contatos.value = contatos.value.filter((item) => item.id !== id);
  } catch (error) {
    console.error('Erro ao deletar o usuário:', error);
  }
};
</script>

<template>
  <section class="container">
    <h1 class="title__main">Cadastro simples</h1>
    <form class="form" @submit.prevent="add">
      <input
        class="field"
        v-model="novoNome"
        placeholder="Digite o nome..."
        @keyup.enter="add"
      />
      <input
        class="field"
        v-model="novoEmail"
        placeholder="Email..."
        @keyup.enter="add"
      />
      <button type="submit" class="btn btn__solid">Adicionar</button>
    </form>

    <!-- Lista de registros -->
    <ul class="cards">
      <li v-for="item in contatos" :key="item.id" class="card__user">
        <p class="card__nome">
            {{ item.nome }} 
            </p>
            <p class="card__email"><strong>E-mail:</strong>
          {{ item.email }} </p>
         <button @click="remove(item.id)" class="btn btn__remove">X</button>
      </li>
    </ul>
  </section>
</template>

<style>
body {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
li {
  list-style: none;
}

.container {
   padding: 3%;
   margin: 0 auto;
   max-width: 600px;
}

.title__main{
  padding:1rem;
}

.form{
  display: flex;
  flex-direction: column;
  gap:15px;
}

.field {
  padding: 12px;
  border: 1px solid #CCC;
  border-radius: 5px;
}

.btn {
  border: 0;
  padding: 10px;
  cursor: pointer;
}

.btn__solid {
  background: rgb(105, 105, 234);
  color: white;
}

.cards{
  margin-top: 2rem;
  display: flex;
  flex-direction: column;
  gap:1em;
}

.card__user{
  display:flex;
  align-items: center;
  justify-content: space-between;
  border:1px solid #ccc;
  padding: 1rem;
}

.btn__remove{
  background-color: tomato;
  color: white;
}
</style>
