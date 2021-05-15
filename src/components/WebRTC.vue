<template>
  <div>
    <h2>1. Start your Webcam</h2>
    <div class="videos">
      <Webcam title="Local Stream" id="webcamVideo" :srcObject="localeSource" />
      <Webcam
        title="Remote Stream"
        id="remoteVideo"
        :srcObject="remoteSource"
      />
    </div>

    <button @click="startWebcam" :disabled="startButtonState">Start webcam</button>
    <h2>2. Create a new Call</h2>
    <button @click="startDating" :disabled="callButtonState">Create Call (offer)</button>

    <h2>3. Join a Call</h2>
    <p>Answer the call from a different browser window or device</p>

    <input id="callInput" />
    <button id="answerButton" :disabled="answerButtonState">Answer</button>

    <h2>4. Hangup</h2>

    <button id="hangupButton" :disabled="hangupButtonState">Hangup</button>

    <!-- <script type="module" src="../js/call.js"></script> -->
  </div>
</template>

<script>
import Webcam from "@/components/Webcam.vue";

export default {
  name: "WebRTC",
  data() {
    return {
      servers: {
        iceServers: [
          {
            urls: [
              "stun:stun1.l.google.com:19302",
              "stun:stun2.l.google.com:19302",
            ],
          },
        ],
        iceCandidatePoolSize: 10,
      },
      pc: null,
      localeSource: null,
      remoteSource: null,
      startButtonState: false,
      callButtonState: true,
      answerButtonState: true,
      hangupButtonState: true,
    };
  },
  components: {
    Webcam,
  },
  methods: {
    async startWebcam() {
      var localStream = await navigator.mediaDevices.getUserMedia({
        video: true,
        audio: true,
      });
      var remoteStream = new MediaStream();
      
      localStream.getTracks().forEach((track) => {
        this.pc.addTrack(track, localStream);
      });

      this.pc.ontrack = (event) => {
        event.streams[0].getTracks().forEach((track) => {
          remoteStream.addTrack(track);
        });
      };
      this.localeSource = localStream;
      this.remoteSource = remoteStream;
      
      this.startButtonState = true;
      this.callButtonState = false;
      this.answerButtonState = false;
      this.hangupButtonState = false;
    },
    async startDating() {

    }
  },
  created() {
    this.pc = new RTCPeerConnection(this.servers);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
