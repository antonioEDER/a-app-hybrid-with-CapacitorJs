<template>
  <div>
    <p>
      <button @click="takePicture()">Fotografar</button>
    </p>
    <div>
      <img v-if="src" :src="src" alt="" height="350" />
      <img v-else src="./assets/def.png" alt="" height="350" />
    </div>
    <br>
    <div>
      <button expand="block" @click="logIn()">
        <img class="google" :src="require('@/assets/google.svg')" />
        <span>Login with Google</span>
      </button>
      <div v-if="user">
        Seus dados: {{ user }}
      </div>
      <div v-else>
        <p>Você não está logado</p>
      </div>
      <div v-if="erro">
        Algo deu errado: {{ erro }}
      </div>
    </div>
    <div>Versão 1.0.2</div>
  </div>
</template>

<script>
import { defineComponent } from "vue";

import { Camera, CameraResultType } from "@capacitor/camera";
import { Toast } from "@capacitor/toast";
import { Haptics } from "@capacitor/haptics";

import { GoogleAuth } from "@reslear/capacitor-google-auth";

export default defineComponent({
  name: "App",
  data() {
    return {
      src: "",
      user: '',
      erro: ''
    };
  },
  mounted() {
    GoogleAuth.initialize({
        clientId:
          "892672575305-fles2fspmcfrirakngd4c3qobca2hth2.apps.googleusercontent.com",
        grantOfflineAccess: true,
        scopes: [
        "profile",
        "email",
        "https://www.googleapis.com/auth/calendar.events",
        "https://www.googleapis.com/auth/contacts.readonly"
      ],
      });
  },
  methods: {
    async logIn (){
      try {
        const response = await GoogleAuth.signIn();
        if (response.id) {
          this.user = response
          this.src = response.imageUrl
          this.hapticsVibrate()
        }
      } catch (e) {
        this.erro = e
        console.log("erro == ", e);
      }
    },
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
});
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
