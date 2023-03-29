<template>
  <div id="voice-component-root">
    <button @click="startRecording()" v-if="!Recording" class="btn btn-success">
      Unmute
    </button>
    <button
      @click="stopRecording()"
      v-else-if="Recording"
      class="btn btn-danger"
    >
      Mute
    </button>
  </div>
</template>

<script>
import axios from "axios";
import Recorder from "recorder-js";

const audioContext = new window.AudioContext();

const recorder = new Recorder(audioContext);

const headers = {
  "Content-Type": "application/json",
};

export default {
  name: "VoiceComponent",
  data() {
    return {
      Recording: false,
    };
  },
  async mounted() {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    recorder.init(stream);

    if (!this.Recording) {
      this.startRecording();
      const interval = setInterval(() => {
        this.stopRecording();
        this.startRecording();
        this.getFromS3();
      }, 1000);
      return () => clearInterval(interval);
    }
  },
  methods: {
    startRecording() {
      this.Recording = true;
      recorder.start();
    },
    stopRecording() {
      this.Recording = false;
      recorder.stop().then(({ blob }) => {
        this.uploadToS3(blob);
      });
    },
    async getBase64Data(data) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(data);
        reader.onload = () => {
          const spliced = reader.result.split(",");
          const header = spliced[0];
          spliced.shift();
          resolve({
            header: header,
            body: spliced.join(""),
          });
        };
        reader.onerror = (err) => {
          console.log("Error: " + err);
          reject(err);
        };
      });
    },
    async uploadToS3(data) {
      return this.getBase64Data(data)
        .then((base64data) => {
          return {
            header: base64data.header,
            base64: base64data.body,
          };
        })
        .then((body) => {
          return body;
        })
        .then((body) => {
          return axios.post(
            "https://pmdwxcp4n1.execute-api.ap-southeast-2.amazonaws.com/dev/voice",
            JSON.stringify(body),
            headers
          );
        });
    },
    async getFromS3() {
      axios
        .post(
          "https://pmdwxcp4n1.execute-api.ap-southeast-2.amazonaws.com/dev/getvoice",
          headers
        )
        .then((result) => {
          console.log(result);
        });
    },
  },
};
</script>

<style scoped></style>
