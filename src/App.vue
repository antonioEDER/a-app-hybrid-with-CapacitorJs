<template>
  <div>
    <p>
      <button @click="takePicture()">Fotografar</button>
    </p>
    <div>
      <img v-if="src" :src="src" alt="" height="350" />
      <img v-else src="./assets/def.png" alt="" height="350" />
    </div>
  </div>
</template>

<script>
import { Camera, CameraResultType } from "@capacitor/camera";
import { Toast } from "@capacitor/toast";

import { Haptics } from '@capacitor/haptics';


export default {
  name: "App",
  data() {
    return {
      src: "",
    };
  },
  methods: {
    async hapticsVibrate() {
      await Haptics.vibrate();
    },
    async takePicture() {
      const image = await Camera.getPhoto({
        quality: 90,
        allowEditing: true,
        resultType: CameraResultType.Uri,
      });
      this.hapticsVibrate();
      this.showToast("Fotografado com sucesso!");

      // image.webPath will contain a path that can be set as an image src.
      // You can access the original file using image.path, which can be
      // passed to the Filesystem API to read the raw data of the image,
      // if desired (or pass resultType: CameraResultType.Base64 to getPhoto)
      var imageUrl = image.webPath;

      // Can be set to the src of an image now
      this.src = imageUrl;
    },
    async showToast(text) {
      await Toast.show({
        text: text,
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
