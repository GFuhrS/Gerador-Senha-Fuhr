<template>
  <div class="senha-container">
    <strong>{{ mostrarSenhas ? senha : senhaOculta }}</strong>
    <button class="toggle" @click="$emit('alternar-visualizacao')">
      {{ mostrarSenhas ? 'Ocultar' : 'Mostrar' }}
    </button>
    <button class="copiar" @click="copiar">Copiar</button>
    <button class="salvar" @click="$emit('salvar')">Salvar</button>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  senha: String,
  mostrarSenhas: Boolean
})

const senhaOculta = computed(() => '•'.repeat(props.senha.length))
const copiar = () => navigator.clipboard.writeText(props.senha)
</script>