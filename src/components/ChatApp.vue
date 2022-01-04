<template>
  <div class="parent-container">
    <div v-if="!joined" class="name-container">
      <input
        type="text"
        class="user-name"
        placeholder="User name..."
        v-model="currentUser"
        v-on:keyup.enter="join"
      />
      <button class="join-button" @click="join">Join</button>
    </div>

    <div v-if="joined" class="chat-interface">
      <h1>Welcome {{ currentUser }}</h1>
      <h3 @click="joined = false">Logout</h3>
      <div class="messages">
        <div v-for="message in messages" :key="message.id">
          <strong>{{ message.user }}</strong>
          : {{ message.text }}
        </div>
      </div>
      <div class="message-field">
        <textarea
          class="message-input"
          name="messages"
          id="messages"
          v-model="text"
          rows="5"
          v-on:keyup.enter="addMessage"
        ></textarea>
      </div>
    </div>
  </div>
</template>

<script>
import io from "socket.io-client";
export default {
  name: "chatApp",
  data() {
    return {
      joined: false,
      currentUser: "",
      text: "",
      messages: [],
    };
  },

  methods: {
    join() {
      this.joined = true;
      this.socketInstance = io("http://localhost:3000");

      this.socketInstance.on("message:received", (data) => {
        this.messages = [data, ...this.messages];
      });
      const message = {
        id: new Date().getTime(),
        text: "has joined",
        user: this.currentUser,
      };

      this.socketInstance.emit("message", message);
    },
    addMessage() {
      const striped = this.text.replace(/<[^>]+>/g, "");
      const message = {
        id: new Date().getTime(),
        text: striped,
        user: this.currentUser,
      };

      this.messages = [message, ...this.messages];
      this.text = "";

      this.socketInstance.emit("message", message);
    },
  },
  props: {
    msg: String,
  },
};
</script>

<style scoped>
.parent-container {
  width: 100%;
  height: 80vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.chat-interface {
  padding: 5px;
  width: 80vw;
}

.messages {
  border: thin solid grey;
  height: 60vh;
  overflow: auto;
  font-size: 18px;
}
.message-field {
  height: 20vh;
  border: thin solid red;
  overflow: hidden;
}

.message-input {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
}

.user-name {
  height: 30px;
  font-size: 20px;
  padding: 5px;
  margin-bottom: 5px;
  text-align: center;
}
.join-button {
  padding: 8px;
  font-size: 18px;
}
.name-container {
  display: flex;
  flex-direction: column;
}
</style>
