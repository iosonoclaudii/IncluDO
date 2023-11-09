<template>
  <div id="app" class="container">
    <div class="left-side">
      <ContactSidebar :coaches="coaches" @coach-selected="showConversation" />
    </div>
    <div class="chat-wrapper">
      <ChatWindow :conversations="selectedConversation" />
      <MessageInput
        :selectedConversation="selectedConversation"
        @user-message-sent="sendMessage"
        @coach-message-sent="sendCoachMessage"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ChatWindow from "./components/ChatWindow.vue";
import MessageInput from "./components/MessageInput.vue";
import ContactSidebar from "./components/ContactSidebar.vue";

export default {
  name: "App",
  components: {
    ChatWindow,
    MessageInput,
    ContactSidebar,
  },
  data() {
    return {
      coaches: [],
      selectedConversation: [],
      coachMessages: [],
    };
  },
  methods: {
    sendMessage(message) {
      if (message.trim() !== "") {
        this.selectedConversation.push({
          id: Date.now(),
          sender: "Utente",
          message: message,
          status: "sent",
        });

        if (this.selectedConversation.length > 0) {
          const lastMessage =
            this.selectedConversation[this.selectedConversation.length - 1];
          setTimeout(() => {
            this.selectedConversation.push({
              id: Date.now() + 1,
              sender: lastMessage.sender === "Utente" ? "Coach" : "Utente",
              message: "Ciao! grazie per avermi scritto.",
              status: "received",
            });
          }, 5000);
        }
      }
    },
    sendCoachMessage(message) {
      this.coachMessages.push({
        id: Date.now() + 1,
        sender: "Coach",
        message: message,
        status: "received",
      });
    },
    showConversation(coach) {
      this.selectedConversation = coach.messages ? coach.messages : [];
    },
  },
  mounted() {
    axios
      .get("https://ott-fogliata.github.io/vuejs-s2i-repository/chat.json")
      .then((response) => {
        this.coaches = response.data;
      })
      .catch((error) => {
        console.error(error);
      });
  },
};
</script>

<style>
@import "./assets/css/global.css";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
