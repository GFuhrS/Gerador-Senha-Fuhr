<template>
  <div class="historico">
    <h3>Senhas Salvas</h3>
    <ul>
      <li v-for="(s, index) in senhas" :key="index">
        {{ mostrarSenhas ? s : '•'.repeat(s.length) }}
        <div class="botoes">
          <button class="copiar" @click="$emit('copiar', s)">Copiar</button>
          <button class="excluir" @click="$emit('excluir', s)">Excluir</button>
        </div>
      </li>
    </ul>
    <button class="limpar" @click="$emit('limpar')">Limpar Histórico</button>
  </div>
</template>


<script setup> // acima loop sobre o array senhas, key=index chave do vue pra identificar cada
// mostra senha reais ou ***, com uma lista n ordenada 

defineProps({ // define oq recebe do comp pai, no caso senhas como array e mostrar boleano
  senhas: Array,
  mostrarSenhas: Boolean, 
})
defineEmits(['copiar', 'excluir', 'limpar']) // define os eventos q o comp pode fazer
</script>

