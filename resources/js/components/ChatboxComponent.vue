<template>
    <div class="chatbox p-3">
        <div class="messages" v-if="messages.length">
            <div class="message" v-for="message in messages" :key="message.id">
                <span class="d-inline-block">{{ message }}</span>
            </div>
        </div>

        <div class="row mt-5">
            <div class="col-3">
                <input type="text" class="form-control" v-model="textMessage" />
            </div>
        </div>

        <div class="row mt-2">
            <div class="col">
                <button class="btn btn-primary" @click="sendMessage()">Send</button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                textMessage: '',
                messages: [],
            }
        },
        created() {
            this.addMessage('You joined the chat.');
            Echo.channel('chatbox')
                .listen('MessageSend', (e) => {
                    this.addMessage(e.message);
                });
        },
        methods: {
            addMessage(message) {
                let date = new Date();
                let timestamp = date.getHours() + ':' + date.getMinutes() + ':' + date.getSeconds();
                this.messages.push(timestamp + ' ' + message);
            },
            sendMessage() {
                axios.post('127.0.0.1:8000/api/message', {message: this.textMessage});
                this.textMessage = '';
            }
        }
    }
</script>