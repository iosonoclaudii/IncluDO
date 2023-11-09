<template>
  <div class="chat-container">
    <ul>
      <li
        v-for="message in conversations"
        :key="message.id"
        :class="
          message.status === 'received' ? 'message-left' : 'message-right'
        "
      >
        {{ message.message }}
        <span v-if="message.status === 'sent'">&#10004;&#10004;</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "ChatWindow",
  props: {
    conversations: {
      type: Array,
      required: true,
    },
  },
};
</script>
