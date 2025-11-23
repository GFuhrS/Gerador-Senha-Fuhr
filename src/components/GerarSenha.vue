<template>
  <div class="generator">
    <label for="tamanho">Tamanho da senha: {{ tamanho }}</label>
    <input type="range" id="tamanho" v-model="tamanho" min="4" max="32" />

    <label><input type="checkbox" v-model="maiusculas" /> Incluir letras maiúsculas</label>
    <label><input type="checkbox" v-model="numeros" /> Incluir números</label>
    <label><input type="checkbox" v-model="simbolos" /> Incluir símbolos</label>

    <div class="button-group">
      <button @click="gerarSenha">Gerar Senha</button>
    </div>
  </div>
</template> 

<script setup> // Acima o teamplate da parte de gerar a senha, checkbox, tamanho, slider etc

import { ref } from 'vue' // funçao vo vue pra criar var reativas (muda o teamplate atualiza)

const emit = defineEmits(['senha-gerada']) // emite evento pro componente pai, sempre q senha é gerada ele envia

const tamanho = ref(12)     //criação das variaveis reativas 
const simbolos = ref(true)
const numeros = ref(true)
const maiusculas = ref(true)

const gerarSenha = () => { // função que é chamada quando o botão é clicado
  const minusculas = 'abcdefghijklmnopqrstuvwxyz'
  const nums = numeros.value ? '0123456789' : ''  // só adiciona num, mais,simb etc se os boleanos tiverem true (checkbox)
  const maius = maiusculas.value ? 'ABCDEFGHIJKLMNOPQRSTUVWXYZ' : ''
  const simb = simbolos.value ? '!@#$%^&*()-_=+[]{};:,.<>?' : ''
  const chars = minusculas + nums + maius + simb

  if (!chars.length) { // se o usuario não marcar nenhuma opção fica vazio
    emit('senha-gerada', 'Selecione ao menos um tipo!')
    return
  }

  let senha = '' // inicializa a senha ('vazia')
  for (let i = 0; i < tamanho.value; i++) { // repete o tamanho enquanto for menor qo  valor
    senha += chars[Math.floor(Math.random() * chars.length)] // escolhe um caracter 'aleatorio' com o math
  }
  emit('senha-gerada', senha) // emite o evento pro componente receber a senha
}
</script>