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

<script setup>

import { ref } from 'vue'

const emit = defineEmits(['senha-gerada'])

const tamanho = ref(12)
const simbolos = ref(true)
const numeros = ref(true)
const maiusculas = ref(true)

const gerarSenha = () => {
  const minusculas = 'abcdefghijklmnopqrstuvwxyz'
  const nums = numeros.value ? '0123456789' : ''
  const maius = maiusculas.value ? 'ABCDEFGHIJKLMNOPQRSTUVWXYZ' : ''
  const simb = simbolos.value ? '!@#$%^&*()-_=+[]{};:,.<>?' : ''
  const chars = minusculas + nums + maius + simb

  if (!chars.length) {
    emit('senha-gerada', 'Selecione ao menos um tipo!')
    return
  }

  let senha = ''
  for (let i = 0; i < tamanho.value; i++) {
    senha += chars[Math.floor(Math.random() * chars.length)]
  }
  emit('senha-gerada', senha)
}
</script>