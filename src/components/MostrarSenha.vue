<template>
  <div class="senha-container">
    <strong>{{ mostrarSenhas ? senha : senhaOculta }}</strong>
    <div class="buttonSenha">
    <button class="toggle" @click="$emit('alternar-visualizacao')">
      {{ mostrarSenhas ? 'Ocultar' : 'Mostrar' }}
     </button>
    <button class="copiar" @click="copiar">Copiar</button>
    <button class="salvar" @click="$emit('salvar')">Salvar</button>
  </div>
</div>
</template>

<script setup> // se mostrarSenhas é true mostra a senha real, senao mostra a oculta só ****
import { computed } from 'vue'

const props = defineProps({ // pega a senha gerada como string + boleano pra mostrar/n mostrar
  senha: String,
  mostrarSenhas: Boolean
})

const senhaOculta = computed(() => '•'.repeat(props.senha.length)) // retorna uma string de bolinhas do msm tamanho
const copiar = () => navigator.clipboard.writeText(props.senha) //função q copia a senha real usando navigator 
</script>
