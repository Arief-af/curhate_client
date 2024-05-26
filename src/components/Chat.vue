<template>

<div class="flex items-start gap-2.5">
   <div :class="self ? 'bg-blue-600 text-white': 'rounded-e-xl dark:bg-gray-700'" class="flex flex-col w-full max-w-[320px] leading-3 p-4 border-gray-200 bg-gray-100 rounded-es-xl">
      <div :class="img ? 'mb-2' : ''" class="flex items-center space-x-2 rtl:space-x-reverse">
         <span class="text-sm font-semibold text-gray-900 dark:text-white">{{ name }}</span>
      </div>
      <div class="w-full" v-if="img">
        <img :src="`https://pq6x4w4q-3000.asse.devtunnels.ms/${img}`" alt="img">
      </div>
      <p class="text-sm font-normal py-2.5 text-gray-900 dark:text-white">{{ message }}</p>
   </div>
</div>
</template>

<script setup>
const props = defineProps({
  name: String,
  message: String,
  self: Boolean,
  img: String
})
</script>

<style lang="scss" scoped>

</style>