<template>
    <div class="contacts-list">
        <ul>
            <li
                v-for="contact in sortedContacts"
                :key="contact.id"
                @click="selectContact(contact)"
                :class="{ selected: contact == selected }"
            >
                <div class="avatar">
                    <img :src="contact.profile_img" :alt="contact.name" />
                </div>
                <div class="contact">
                    <p class="name">{{ contact.name }}</p>
                    <p class="email">{{ contact.email }}</p>
                </div>
                <span class="unread" v-if="contact.unread">
                    {{ contact.unread }}
                </span>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "ContactList",

        props: {
            contacts: {
                type: Array,
                default: []
            }
        },

        data: function() {
            return {
                selected: this.contacts.length ? this.contacts[0] : null
            };
        },

        methods: {
            selectContact: function(contact) {
                this.selected = contact;
                this.$emit("selected", contact);
            }
        },

        computed: {
            sortedContacts: function() {
                return _.sortBy(this.contacts, [
                    contact => {
                        if (contact == this.selected) {
                            return Infinity;
                        }

                        return contact.unread;
                    }
                ]).reverse();
            }
        }
    };
</script>

<style lang="scss" scoped>
.contacts-list {
    flex: 2;
    max-height: 600px;
    overflow: scroll;
    border-left: 1px solid #a6a6a6;

    ul {
        list-style-type: none;
        padding-left: 0;

        li {
            display: flex;
            padding: 2px;
            border-bottom: 1px solid #aaa;
            height: 80px;
            position: relative;
            cursor: pointer;

            &.selected {
                background: #dfdfdf;
            }

            span.unread {
                background: #7ee17e;
                position: absolute;
                top: 20px;
                right: 10px;
                min-width: 20px;
                display: flex;
                justify-content: center;
                align-items: center;
                font-size: 12px;
                padding: 0 4px;
                border-radius: 3px;
                color: #fff;
            }

            .avatar {
                flex: 1;
                display: flex;
                align-items: center;

                img {
                    width: 35px;
                    border-radius: 50%;
                    margin: 0 auto;
                }
            }

            .contact {
                flex: 3;
                font-size: 10px;
                overflow: hidden;
                display: flex;
                flex-direction: column;
                justify-content: center;

                p {
                    margin: 0;

                    &.name {
                        font-weight: bold;
                    }
                }
            }
        }
    }
}
</style>