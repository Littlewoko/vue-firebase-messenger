<template>
  <form class="w-full max-w-sm" @submit.prevent="handleFormSubmit">
    <div class="md:flex md:items-center mb-6">
      <div class="md:w-1/3">
        <label
          class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
          for="id-message"
        >
          Message
        </label>
      </div>
      <div class="md:w-2/3">
        <input
          class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500"
          id="id-message"
          type="text"
          placeholder="Message"
          v-model="text"
        />
      </div>
    </div>
    <div class="md:flex md:items-center mb-6">
      <div class="md:w-1/3">
        <label
          class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
          for="id-nickname"
        >
          Nickname
        </label>
      </div>
      <div class="md:w-2/3">
        <input
          class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500"
          id="id-nickname"
          type="text"
          placeholder="Nickname"
          v-model="nickname"
        />
      </div>
    </div>
    
    <div class="md:flex md:items-center">
      <div class="md:w-1/3"></div>
      <div class="md:w-2/3">
        <button
          class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
          type="button"
          @click="handleFormSubmit"
        >
          Send
        </button>
      </div>
    </div>
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";

const emit = defineEmits(["message"])

const text = ref("");
const nickname = ref("");

const handleFormSubmit = () => {
    console.log(`${text.value} from ${nickname.value}`)

    emit("message", { text: text.value, nickname: nickname.value });
    
    text.value = "";
}

</script>

<style>
</style>