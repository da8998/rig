<template>
  <div id="home-component-root">
    <div id="home-component-apps">
      <button @click="changeComponent('Voice')" id="voice-button" class="btn btn-rounded">
        <font-awesome-icon id="voice-icon" icon="fa-solid fa-headset" />
      </button>
    </div>

    <VoiceComponent v-if="showVoiceComponent" />
  </div>
</template>

<script>
import VoiceComponent from "../voice/VoiceComponent.vue";

export default {
  name: "HomeComponent",
  components: {
    VoiceComponent
  },
  data() {
    return {
      showVoiceComponent: false,
    };
  },
  methods: {
    changeComponent(component) {
      if (component === "Voice") {
        this.showVoiceComponent = !this.showVoiceComponent;
      }
    },
  },
};
</script>

<style scoped>
#home-component-root {
  background-color: rgb(15, 15, 15);
  color: white;
  position: fixed;
  width: 100%;
  height: 100%;
  margin: auto;
  text-align: center;
  scroll-behavior: smooth;
}
#home-component-apps {
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  position: fixed;
  width: 100%;
  height: 70px;
  bottom: 0;
  right: 0;
  text-align: center;
}
button {
  display: inline;
  height: 70px;
  text-align: center;
}
#voice-icon {
  width: 35px;
  height: 35px;
}
</style>
