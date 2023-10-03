<template>
  <center>
    <div style="width: 50%">
      <br/>
      <center>
        <button
          v-if="cameraOpen === false"
          @click="bukaKamera"
          style="width: 50%"
          
        >
        𝘽𝙐𝙆𝘼 𝙆𝘼𝙈𝙀𝙍𝘼
        </button>
        <button
          v-else
          style="width: 64%"
          @click="tutupKamera"

        >
        𝑻𝑼𝑻𝑼𝑷 𝑲𝑨𝑴𝑬𝑹𝑨
        </button>
        <br/>
        <img
          v-if="ulangis === false"
          style="transform: scaleX(-1)"
          :src="imgSrc"
        />
        <img v-else :src="imgSrc" style="transform: scaleX(-1)" /><br />
        <div v-if="cameraOpen == true">
          <video
            v-if="cameras == true"
            ref="videoElement"
            autoplay
            style="border-radius: 20px; transform: scaleX(-1)"
          ></video>
        </div>
        <br />
        <span v-if="cameras == true">
          <button
            v-if="ambilscrinsut == true"
            @click="ulangi"
            style="width: 32%"
           
          >
          𝑹𝑬𝑭𝑬𝑨𝑻
          </button>
        </span>

        <div v-if="cameras == true">
          <span>
            <button
              v-if="ambilscrinsut == false"
              @click="ambilScreenshot"
              style="width: 32%"
            >
            𝑨𝑴𝑩𝑰𝑳 𝑷𝑯𝑶𝑻𝑶
              
            </button>
            &nbsp;
          </span>
          
        </div>
        <br>
      </center>
    </div>
  </center>
</template>
<script>
export default {
  data() {
    return {
      stream: null,
      cameras: false,
      imgSrc: null,
      cameraOpen: false,
      ulangis: false,
      ambilscrinsut: false,
    };
  },
  methods: {
    bukaKamera() {
      this.imgSrc = null;
      this.cameras = true;
      this.cameraOpen = true;
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then((stream) => {
          this.stream = stream;
          this.$refs.videoElement.srcObject = stream;
        })
        .catch((error) => {
          alert("Error accessing camera:", error);
        });
    },
    tutupKamera() {
      if (this.stream) {
        this.stream.getTracks().forEach((track) => track.stop());
        this.$refs.videoElement.srcObject = null;
      }
      this.cameraOpen = false;
      this.cameras = false;
    },
    ambilScreenshot() {
      const video = this.$refs.videoElement;
      const canvas = document.createElement("canvas");
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      const context = canvas.getContext("2d");
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      const dataURL = canvas.toDataURL("image/png");
      this.imgSrc = dataURL;
      this.tutupKamera();
      this.cameras = true;
      this.ambilscrinsut = true;
    },
    ulangi() {
      this.imgSrc = null; // Menghapus gambar yang ada
      this.bukaKamera();
      this.ulangis = true;
      this.ambilscrinsut = false;
    },
  },
};
</script>