<template>
  <div id="ap">
    <h2>Gerador de Senhas</h2>
    <p>Observe a força da sua senha! Quanto mais caracteres, números e símbolos, mais forte ela será!</p>

    <PasswordGenerator 
      @senha-gerada="atualizarSenha"
    /> <!--  componente que gera senhas, esperando o comp filho enviar p atualizar senha-->


    <PasswordDisplay 
      v-if="senhaGerada"
      :senha="senhaGerada"
      :mostrarSenhas="mostrarSenhas"
      @alternar-visualizacao="mostrarSenhas = !mostrarSenhas"
      @salvar="salvarSenha"
    /><!--mostra senha individualmente, só exibe se tiver uma gerada, controla se aparece ou nao
    se o usuario da o click em mostrar/ocultar altera o boleano e chama salvar senha --> 

    <PasswordStrength 
      v-if="senhaGerada"
      :senha="senhaGerada"
    /> <!-- recebe a senha gerada e só mostra a força se tiver alguma gerada-->

    <SavedPasswords 
      v-if="senhasSalvas.length"
      :senhas="senhasSalvas"
      :mostrarSenhas="mostrarSenhas" 
      @copiar="copiarSenha"
      @excluir="excluirSenha"
      @limpar="limparSenhas"
    />
  </div> <!--exibe as salvas, envia o array pro componente, controla se exibe/oculta e eventos do pai pra ocultar etc.. -->
</template>

<script setup>
import { ref, onMounted } from 'vue'
import PasswordGenerator from './components/GerarSenha.vue'
import PasswordDisplay from './components/MostrarSenha.vue'
import PasswordStrength from './components/ForcaSenha.vue'
import SavedPasswords from './components/SenhasSalvas.vue'
// importa os filhos

const senhaGerada = ref('') // guarda senha atual
const senhasSalvas = ref([]) // array de todas salvas
const mostrarSenhas = ref(true) // controla se aparece ou n

onMounted(() => { // quando o app carrega busca se tem alguma senha no local storage
  const local = localStorage.getItem('senhas') 
  if (local) senhasSalvas.value = JSON.parse(local)
}) // se encontrar transforma o JSON em array /\ e guarda em senhas salvas

const atualizarSenha = (novaSenha) => senhaGerada.value = novaSenha 
// atualiza sempre q o componente gerador emitir o evento 

const salvarSenha = () => { // salva senha, evita duplicatas e atualiza o JSOn do local storage
  if (senhaGerada.value && !senhasSalvas.value.includes(senhaGerada.value)) {
    senhasSalvas.value.unshift(senhaGerada.value) // unshift pra fica acima da ultima
    localStorage.setItem('senhas', JSON.stringify(senhasSalvas.value))
  }
}

const copiarSenha = (senha) => navigator.clipboard.writeText(senha) // copia c o nav clip pra area de transferencia

const excluirSenha = (senha) => { // remove uma senha e atualiza o local
  senhasSalvas.value = senhasSalvas.value.filter(s => s !== senha)
  localStorage.setItem('senhas', JSON.stringify(senhasSalvas.value))
}

const limparSenhas = () => { // limpa tudo
  senhasSalvas.value = []
  localStorage.removeItem('senhas')
}
</script>