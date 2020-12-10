<template>
    <div class="chat-app">
        <Conversation :contact="selectedContact" :messages="messages" />
        <ContactList :contacts="contacts" @selected="startConversationWith" />
    </div>
</template>

<script>
    import Conversation from "./Conversation";
    import ContactList from "./ContactList";
    export default {
        name: "ChatApp",

        props: {
            user: {
                type: Object,
                required: true
            }
        },

        data: function() {
            return {
                selectedContact: null,
                messages: [],
                contacts: []
            };
        },

        mounted: function() {
            axios.get("/contacts").then(response => {
                this.contacts = response.data;
            });
        },

        methods: {
            startConversationWith: function(contact) {
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
