<template>
  <div>
        <span>
            <h3>{{title}}</h3>
            <video :id="id" autoplay playsinline></video>
        </span>
  </div>
</template>

<script>
export default {
  name: 'WebRTC',
  props: {
    title: String,
    id: String,
    srcObject: Object,
  },
  data() {
      return {
        webcamVideo: null,      
      };
  },
  updated() {
      console.log("Updated " + this.title)
    this.webcamVideo = document.getElementById(this.id);
    this.webcamVideo.srcObject = this.srcObject;
  }
}
</script>

<style>

</style>