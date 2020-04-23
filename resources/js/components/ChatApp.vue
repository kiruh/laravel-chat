<template>
  <div class="chat-app">
    <Conversation :contact="selectedContact" :messages="messages" />
    <ContactList :contacts="contacts" @selected="startConversation" />
  </div>
</template>

<script>
import Conversation from "./Conversation";
import ContactList from "./ContactList";

export default {
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      selectedContact: null,
      messages: [],
      contacts: []
    };
  },
  mounted() {
    console.log(this.user);
    axios.get("/contacts").then(response => {
      console.log(response);
      this.contacts = response.data;
    });
  },
  methods: {
    startConversation(contact) {
      axios.get(`/conversation/${contact.id}`).then(response => {
        this.messages = response.data;
        this.selectedContact = contact;
      });
    }
  },
  components: { Conversation, ContactList }
};
</script>

<style lang="scss" scoped>
.chat-app {
  display: flex;
}
</style>