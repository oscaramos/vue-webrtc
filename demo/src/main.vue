<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12 my-3">
        <h2>Room</h2>
        <input v-model="roomId">
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <div class="">
          <vue-webrtc ref="webrtc"
                      width="100%"
                      :roomId="roomId"
                      enable-chat
                      hide-chat
                      v-on:joined-room="logEvent"
                      v-on:left-room="logEvent"
                      v-on:opened-room="logEvent"
                      v-on:share-started="logEvent"
                      v-on:share-stopped="logEvent"
                      v-on:sent-message="logEvent"
                      v-on:received-message="handleReceivedMessage"
                      @error="onError" />
        </div>
        <div class="row">
          <div class="col-md-12 my-3">
            <button type="button" class="btn btn-primary" @click="onJoin">Join</button>
            <button type="button" class="btn btn-primary" @click="onLeave">Leave</button>
            <button type="button" class="btn btn-primary" @click="onCapture">Capture Photo</button>
            <button type="button" class="btn btn-primary" @click="onShareScreen">Share Screen</button>
          </div>
        </div>
        <div>
          <h2>Controlled chat</h2>
          <h4>Messages</h4>
          <div v-for="message in messages">
            {{message}}
          </div>
          <input type="text" v-model="message">
          <button type="button" class="btn" @click="onSendMessage">Send</button>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <h2>Captured Image</h2>
        <figure class="figure">
          <img :src="img" class="img-responsive" />
        </figure>
      </div>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import { WebRTC } from 'plugin';

  import * as io from 'socket.io-client'
  window.io = io

  Vue.component(WebRTC.name, WebRTC);

  export default {
    name: 'app',
    components: {
    },
    data() {
      return {
        img: null,
        roomId: "public-room-chat",
        messages: [],
      };
    },
    computed: {
    },
    watch: {
    },
    methods: {
      onCapture() {
        this.img = this.$refs.webrtc.capture();
      },
      onJoin() {
        this.$refs.webrtc.join();
      },
      onLeave() {
        this.$refs.webrtc.leave();
      },
      onShareScreen() {
        this.img = this.$refs.webrtc.shareScreen();
      },
      onError(error, stream) {
        console.log('On Error Event', error, stream);
      },
      onSendMessage() {
        console.log(`sent: ${this.message}`);
        this.$refs.webrtc.sendMessage(this.message);
      },
      handleReceivedMessage(message) {
        console.log(`received: ${message}`);
        this.messages.push(message)
      },
      logEvent(event) {
        console.log('Event : ', event);
      },
    }
  };
</script>
