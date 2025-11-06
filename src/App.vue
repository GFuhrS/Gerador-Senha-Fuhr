<template>
  <div id="ap">
    <h2>Gerador de Senhas</h2>
    <p>Observe a força da sua senha! Quanto mais caracteres, números e símbolos, mais forte ela será!</p>

    <PasswordGenerator 
      @senha-gerada="atualizarSenha"
    />


    <PasswordDisplay 
      v-if="senhaGerada"
      :senha="senhaGerada"
      :mostrarSenhas="mostrarSenhas"
      @alternar-visualizacao="mostrarSenhas = !mostrarSenhas"
      @salvar="salvarSenha"
    />

    <PasswordStrength 
      v-if="senhaGerada"
      :senha="senhaGerada"
    />

    <SavedPasswords 
      v-if="senhasSalvas.length"
      :senhas="senhasSalvas"
      :mostrarSenhas="mostrarSenhas" 
      @copiar="copiarSenha"
      @excluir="excluirSenha"
      @limpar="limparSenhas"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import PasswordGenerator from './components/GerarSenha.vue'
import PasswordDisplay from './components/MostrarSenha.vue'
import PasswordStrength from './components/ForcaSenha.vue'
import SavedPasswords from './components/SenhasSalvas.vue'

const senhaGerada = ref('')
const senhasSalvas = ref([])
const mostrarSenhas = ref(true)

onMounted(() => {
  const local = localStorage.getItem('senhas')
  if (local) senhasSalvas.value = JSON.parse(local)
})

const atualizarSenha = (novaSenha) => senhaGerada.value = novaSenha

const salvarSenha = () => {
  if (senhaGerada.value && !senhasSalvas.value.includes(senhaGerada.value)) {
    senhasSalvas.value.unshift(senhaGerada.value)
    localStorage.setItem('senhas', JSON.stringify(senhasSalvas.value))
  }
}

const copiarSenha = (senha) => navigator.clipboard.writeText(senha)

const excluirSenha = (senha) => {
  senhasSalvas.value = senhasSalvas.value.filter(s => s !== senha)
  localStorage.setItem('senhas', JSON.stringify(senhasSalvas.value))
}

const limparSenhas = () => {
  senhasSalvas.value = []
  localStorage.removeItem('senhas')
}
</script>