<template>
  <div class="contact-sidebar">
    <h1>IncluDO</h1>
    <input type="text" v-model="searchQuery" placeholder="Cerca contatti" />
    <ul>
      <li
        v-for="coach in filteredCoaches"
        :key="coach.name"
        @click="selectCoach(coach)"
        class="contact-item"
      >
        <div class="name">{{ coach.name }}:</div>
        <div class="messages">
          <span
            v-if="coach.messages && coach.messages.length"
            :style="{
              color:
                coach.messages[coach.messages.length - 1].status === 'received'
                  ? 'gray'
                  : 'green',
            }"
          >
            {{
              limitMessage(coach.messages[coach.messages.length - 1].message)
            }}
          </span>
          <span v-else>Nessun messaggio</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      coaches: [],
      searchQuery: "",
      coachMessages: [],
      messageLimit: 25,
    };
  },
  computed: {
    filteredCoaches() {
      if (this.searchQuery === "") {
        return this.allCoaches;
      } else {
        const lowerCaseQuery = this.searchQuery.toLowerCase();
        return this.allCoaches.filter((coach) =>
          coach.name.toLowerCase().includes(lowerCaseQuery)
        );
      }
    },
    allCoaches() {
      return [
        {
          name: "Coach",
          messages: this.coachMessages,
        },
        ...this.coaches,
      ];
    },
  },
  methods: {
    selectCoach(coach) {
      if (coach.name === "Coach") {
        this.coachMessages = coach.messages ? coach.messages : [];
        this.$emit("coach-selected", {
          name: "Coach",
          messages: this.coachMessages,
        });
      } else {
        this.$emit("coach-selected", coach);
      }
    },
    limitMessage(message) {
      if (message.length > this.messageLimit) {
        return message.substring(0, this.messageLimit) + "...";
      }
      return message;
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
