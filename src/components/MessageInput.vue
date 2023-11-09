<template>
  <div class="input-container">
    <input type="text" v-model="message" />
    <button @click="sendMessage">Invia</button>
  </div>
</template>

<script>
export default {
  name: "MessageInput",
  props: {
    selectedConversation: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      message: "",
    };
  },
  methods: {
    sendMessage() {
      if (this.message.trim() !== "") {
        this.$emit("user-message-sent", this.message);
        this.message = "";

        if (
          this.selectedConversation.length > 0 &&
          this.selectedConversation[this.selectedConversation.length - 1]
            .sender === "Coach"
        ) {
          setTimeout(() => {
            this.$emit(
              "coach-message-sent",
              "Ciao, grazie per avermi scritto."
            );
          }, 5000);
        }
      }
    },
  },
};
</script>

<style scoped>
.input-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
}

input {
  width: calc(100% - 22px);
  padding: 12px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 20px;
}

button {
  background-color: #2c3e50;
  color: white;
  padding: 12px 20px;
  margin-bottom: 10px;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
}

button:hover {
  background-color: #1e2a35;
}
</style>
