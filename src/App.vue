<template>
  <div class="p-20 flex justify-center">
    <MessengerForm @message="handleNewMessage" />
  </div>

  <div class="p-20 flex justify-center">
    <MessengerMessages :messages="messages" @delete="handleDelete" @update="handleUpdate"/>
  </div>
</template>

<script>
import MessengerForm from "./components/MessengerForm.vue";
import MessengerMessages from "./components/MessengerMesages.vue";
import { app } from "./composables/firebaseinit";
import { getDatabase, ref, onValue, set, remove } from "firebase/database";

export default {
  name: "App",
  components: {
    MessengerForm,
    MessengerMessages,
  },
  data() {
    return {
      messages: [],
    };
  },
  methods: {
    handleNewMessage(payload) {
      const database = getDatabase(app);

      const randomId = Math.floor(Math.random() * 1000) // not ideal obviously

      const messagesRef = ref(database, `messages/${randomId}`);

      
      set(messagesRef, {
        text: payload.text,
        nickname: payload.nickname,
      });
    },
    handleDelete(payload) {
      const database = getDatabase(app);

      const messagesRef = ref(database, `messages/${payload.key}`);

      remove(messagesRef);

    },
    handleUpdate({ key, text, nickname}) {
      const database = getDatabase(app);

      const messagesRef = ref(database, `messages/${key}`);
      
      set(messagesRef, {
        text: text,
        nickname: nickname,
      });
    },
    setupListener() {
      const database = getDatabase(app);

      const messagesRef = ref(database, "messages");

      onValue(messagesRef, (snapshot) => { // get db records whenever there is an update
        const data = snapshot.val();
        this.messages = [];
        for (const key in data) {
          const message = {
            key: key, 
            text: data[key].text, 
            nickname: data[key].nickname,
          }

          this.messages.push(message); 
        }
      });
    },
  },
  created() {
    this.setupListener();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
