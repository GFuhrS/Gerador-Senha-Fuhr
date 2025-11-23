<template>
  <div class="forca-container">
    <div class="barra" :style="{ width: forca.percent + '%', backgroundColor: forca.cor }"></div>
    <span class="forca-texto">{{ forca.texto }}</span>
  </div>
</template>

<script setup>
import { computed } from 'vue' // vai ser computado atuomaticamente 
const props = defineProps({ senha: String }) // diz ao vue q esse componente recebe uma senha do comp pai

const forca = computed(() => { 
  const s = props.senha // pega a senha passada 
  let pontos = 0 // começa o contador em 0 
  if (s.length >= 8) pontos++
  if (s.length >= 12) pontos++
  if (/[A-Z]/.test(s)) pontos++ // test pra ''perguntar'' se tem maiusc
  if (/[0-9]/.test(s)) pontos++ // se tem num
  if (/[^a-zA-Z0-9]/.test(s)) pontos++ // tem simb?

  // dependendo do resultado preenche com percent a barra + cor do %
  if (pontos <= 2) return { texto: 'Fraca', percent: 25, cor: '#ff4d4d' }
  if (pontos === 3) return { texto: 'Média', percent: 50, cor: '#f7b83c' }
  if (pontos === 4) return { texto: 'Forte', percent: 75, cor: '#00ac4d' }
  return { texto: 'Muito Forte', percent: 100, cor: '#00ffbf' }
})
</script>