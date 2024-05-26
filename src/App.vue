<template>
  <section v-show="!isLogin" class="w-screen h-screen flex justify-center items-center">
    <main class="w-full px-10">
      <div class="mb-4">
        <input v-model="form.name" type="text" placeholder="Masukan Nama Anda" class="input input-bordered w-full" />
      </div>
      <button class="btn w-full" @click="isLogin = !isLogin">Login</button>
    </main>
  </section>
  <main v-if="isLogin">
    <div class="flex justify-end p-10">
      <button class="btn w-max" @click="isLogin = !isLogin">Logout</button>
    </div>
    <div class="fixed bottom-0 w-full justify-center flex p-10 gap-5">
      <input type="file" ref="fileInputRef" style="display: none" accept="image/*" @change="onFileChange">
      <input v-model="form.message" type="text" placeholder="Masukan Pesan Anda" class="input input-bordered w-full max-w-xs" />
      <button class="btn" @click="openFileInput">Select Image</button>
      <button class="btn" @click="onClick">Send</button>
    </div>
    <section class="mx-2 flex p-10">
      <div class="wrapper w-full">
        <div class="text-red-500" v-for="data in messages" :key="data.id">
          <div :class="data.name == form.name ? 'justify-end' : ''" class="flex  w-full">
            <Chat class="mb-3 w-[320px]" :img="data.imageUrl" :self="data.name == form.name" :name="data.name" :message="data.message" />
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';
import Pusher from 'pusher-js';
import Chat from './components/Chat.vue';

let isLogin = ref(false);
const messages = ref([]);
const form = ref({
  message: '',
  name: ''
});
const fileInputRef = ref(null);

const onFileChange = (event) => {
  fileInputRef.value = event.target.files[0];
};
const openFileInput = () => {
  fileInputRef.value.click();
};

const onClick = async () => {
  try {
    const formData = new FormData();
    formData.append('message', form.value.message);
    formData.append('name', form.value.name);
    if (fileInputRef.value) {
      formData.append('image', fileInputRef.value.files[0]);
    }

    const resp = await axios.post('https://pq6x4w4q-3000.asse.devtunnels.ms/message', formData);
    
    form.value.message = '';
    fileInputRef.value.value = ''; // Reset file input value to clear it
  } catch (error) {
    console.log(error);
  }
};


onMounted(async () => {
  let pusher = new Pusher('f483ab9e753fe87b0e1b', {
    cluster: 'ap1',
  });

  let channel = pusher.subscribe('chat');
  channel.bind('message', function (data) {
    messages.value.push(data);
  });
});
</script>
