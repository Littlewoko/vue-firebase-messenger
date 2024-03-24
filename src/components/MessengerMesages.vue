<template>
  <div class="flex flex-column gap-3">
    <template
      v-for="message in props.messages"
      :key="message.text + ' ' + message.nickname"
    >
      <div class="max-w-sm w-full lg:max-w-full lg:flex">
        <div
          class="border-r border-b border-l border-gray-400 lg:border-l-0 lg:border-t lg:border-gray-400 bg-white rounded-b lg:rounded-b-none lg:rounded-r p-4 flex flex-col justify-between leading-normal"
        >
          <div class="mb-8">
            <template v-if="!update">
              <p class="text-gray-700 text-base">
                {{ message.text }}
              </p>
            </template>
            <template v-else>
              <input
                :id="message.key + ':text'"
                type="text"
                :value="message.text"
              />
            </template>
          </div>
          <div class="flex items-center">
            <template v-if="!update">
              <div class="text-sm">
                <p class="text-gray-900 leading-none">{{ message.nickname }}</p>
                <p class="text-gray-600">Aug 18</p>
              </div>
            </template>
            <template v-else>
              <input
                :id="message.key + ':nickname'"
                type="text"
                :value="message.nickname"
              />
            </template>
          </div>
          <button @click="emitDelete(message.key)">Delete</button>
          <button v-if="update" @click="emitUpdate(message.key)">Update</button>
        </div>
      </div>
    </template>
    <button v-if="props.messages && props.messages.length > 0" @click="update = !update">
        {{ !update ? "Edit" : "Stop Editting" }}
      </button>
    <template v-if="!props.messages || props.messages.length == 0">
      <div
        class="bg-orange-100 border-l-4 border-orange-500 text-orange-700 p-4"
        role="alert"
      >
        <p class="font-bold">No Messages</p>
        <p>Submit a new message</p>
      </div>
    </template>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from "vue";

const update = ref(false);

const props = defineProps(["messages"]);
const emit = defineEmits(["delete"]);

const emitDelete = (key) => {
  emit("delete", { key });
};

const emitUpdate = (key) => {
  const text = document.getElementById(`${key}:text`).value;
  const nickname = document.getElementById(`${key}:nickname`).value;

  emit("update", { key, text, nickname });

  update.value = false;
};
</script>

<style>
</style>