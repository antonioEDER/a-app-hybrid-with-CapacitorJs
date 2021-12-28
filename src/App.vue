<template>
  <div>
    <p>
      <button @click="takePicture()">Fotografar</button>
      <img :src="src" alt="" height="350px" />
    </p>

    <p>
      <button @click="showHelloToast()">Abrir alerta</button>
    </p>
  </div>
</template>

<script>
import { Camera, CameraResultType } from "@capacitor/camera";
import { Toast } from "@capacitor/toast";

export default {
  name: "App",
  data() {
    return {
      src: "",
    };
  },
  methods: {
    async takePicture() {
      const image = await Camera.getPhoto({
        quality: 90,
        allowEditing: true,
        resultType: CameraResultType.Uri,
      });

      // image.webPath will contain a path that can be set as an image src.
      // You can access the original file using image.path, which can be
      // passed to the Filesystem API to read the raw data of the image,
      // if desired (or pass resultType: CameraResultType.Base64 to getPhoto)
      var imageUrl = image.webPath;

      // Can be set to the src of an image now
      this.src = imageUrl;
    },
    async showHelloToast() {
      await Toast.show({
        text: "Olá!",
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
