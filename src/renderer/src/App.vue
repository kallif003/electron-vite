<template>
  <button @click="showNotification" class="bg-slate-800 rounded-xl text-2xl">Notificar</button>
</template>

<script setup>
import { ref } from 'vue'

// Função para verificar permissão e exibir a notificação
function showNotification() {
  // Verificar se o navegador suporta notificações
  if (!('Notification' in window)) {
    alert('Este navegador não suporta notificações.')
    return
  }

  // Pedir permissão ao usuário se ainda não foi concedida
  if (Notification.permission === 'default') {
    Notification.requestPermission().then((permission) => {
      if (permission === 'granted') {
        createNotification()
      } else {
        alert('Permissão para notificações foi negada.')
      }
    })
  } else if (Notification.permission === 'granted') {
    // Se a permissão já foi concedida, mostrar a notificação
    createNotification()
  } else {
    alert('As notificações estão bloqueadas. Ative-as nas configurações do navegador.')
  }
}

// Função para criar e exibir a notificação
function createNotification() {
  const notification = new Notification('Notificação', {
    body: 'Esta é uma notificação de teste!',
    icon: '/icons/icon-192x192.png'
  })

  notification.onclick = () => {
    window.focus()
  }
}
</script>

<style scoped>
button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>
