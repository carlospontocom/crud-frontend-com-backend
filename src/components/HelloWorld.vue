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
    <div>
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
      <button @click="add" class="btn btn__solid">Adicionar</button>
    </div>

    <!-- Lista de registros -->
    <ul>
      <li v-for="item in contatos" :key="item.id">
        <h2>
          {{ item.id }} - {{ item.nome }} - <strong>E-mail:</strong>
          {{ item.email }}
        </h2>
        <button @click="remove(item.id)">X</button>
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
  background: gray;
}

.field {
  padding: 12px;
  border: 0;
  border-radius: 5px;
}

.btn {
  padding: 10px;
  cursor: pointer;
}

.btn__solid {
  background: blue;
  color: white;
}
</style>
