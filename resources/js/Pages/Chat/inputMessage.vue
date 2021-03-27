<template>
    <div class="relative n-1">
        <div class="message-input p-5" style='border-top: 1px solid #e6e6e6;'>
            <input
                type="text"
                v-model="message"
                @keyup.enter="sendMessage"
                placeholder="Say something..."
            />
            <button class="btn" @click="sendMessage">
                Send
            </button>
        </div>
    </div>
</template>

<style scoped>
.message-input {
    display: flex;
    justify-content: space-between;
}
input {
    border: none;
    outline: none;
    width: 70%;
}
input:focus {
  border-color: inherit;
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn {
    background: rgb(36, 36, 36);
    border-radius: 12px;
    color: #fff;
    width: 75px;
}
</style>

<script>
export default {
    name: 'inputMessage',
    props: ['room'],
    data: () => ({
        message: '',
    }),
    methods: {
        sendMessage() {
            if (this.message === '') {
                return;
            }

            axios.post(`/chat/room/${this.room.id}/message`, {
                message: this.message,
            }).then(response => {
                if (response.status == 201) {
                    this.message = '';
                    this.$emit('messageSent');
                }
            }).catch(error => {
                console.log(error);
            })
        }
    }
}
</script>