<template>
    <div class="chat-app">
        <Conversation
            :contact="selectedContact"
            :messages="messages"
            @new="saveNewMessage"
        />
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
        Echo.private(`messages.${this.user.id}`).listen("NewMessage", e => {
            this.handleIncoming(e.message);
        });

        axios.get("/contacts").then(response => {
            this.contacts = response.data;
        });
    },
    methods: {
        startConversation(contact) {
            axios.get(`/conversation/${contact.id}`).then(response => {
                this.messages = response.data;
                this.selectedContact = contact;
            });
        },
        saveNewMessage(message) {
            this.messages.push(message);
        },
        handleIncoming(message) {
            if (
                this.selectedContact &&
                message.from === this.selectedContact.id
            ) {
                this.saveNewMessage(message);
                return;
            }
            alert(message.text);
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
