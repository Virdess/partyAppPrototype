<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-item>
        <ion-textarea v-model="message" label="Ваше сообщение" placeholder="Введите сообщение" ></ion-textarea>
      </ion-item>
      <ion-button @click="sendMessage" expand="block">Отправить</ion-button>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonTextarea, IonItem, IonContent, IonButton } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { io, Socket } from 'socket.io-client';
import { ref, onMounted, onBeforeUnmount } from 'vue';

// Переменная для хранения сообщения
const message = ref('');
const socket: Socket = io('http://localhost:3000'); // Замените на URL вашего сервера

// Отправка сообщения
function sendMessage() {
  if (message.value.trim()) {
    socket.emit('message', { sender: 'User1', message: message.value });
    message.value = ''; // Очистить поле ввода
  }
}

// Подключение к сокету
onMounted(() => {
  socket.on('connect', () => {
    console.log('Подключено к серверу');
  });

  socket.on('message', (data) => {
    console.log('Получено сообщение:', data);
  });

  socket.on('disconnect', () => {
    console.log('Отключено от сервера');
  });
});

// Отключение при удалении компонента
onBeforeUnmount(() => {
  socket.disconnect();
});
</script>

<style>
textarea {
  height: 50vh;
}
</style>