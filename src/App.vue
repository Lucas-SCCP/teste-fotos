<!-- eslint-disable no-undef -->
<!-- eslint-disable no-undef -->
<template>
  <main>
    <!--<nav class="navbar">
      <img src="/src/assets/svg/logo.svg" class="navbar__logo" alt="" />
    </nav>-->
    <div class="slideshow-container">
      <div class="mySlides fade">
        <!--<div class="numbertext">1 / 3</div>-->
        <img src="https://scontent.fbau3-2.fna.fbcdn.net/v/t31.18172-8/11807755_10207587619555136_708705202085441635_o.jpg?_nc_cat=110&ccb=1-7&_nc_sid=cdbe9c&_nc_eui2=AeHlzzP3GA0R8QCzDfkuFzk5S-7khg6fPQ9L7uSGDp89D9MlMTmyAMu09wLnSJ9uFCI&_nc_ohc=5P8LkBK2qz0AX_ScpJy&_nc_ht=scontent.fbau3-2.fna&oh=00_AfD3LBagO4AqLBJhKKl297sA85WaGsHdLKQNOLOiCdOo9g&oe=64C81003" style="width:100%">
        <!--<div class="text">Caption Text</div>-->
      </div>
      <div class="mySlides fade">
        <!--<div class="numbertext">2 / 3</div>-->
        <img src="https://scontent.fbau3-2.fna.fbcdn.net/v/t31.18172-8/11059721_10207691591394367_7159485778453789867_o.jpg?_nc_cat=110&ccb=1-7&_nc_sid=cdbe9c&_nc_eui2=AeHYAg_le6jJpTVNQnRdhD4-O6We1Hsf-Mc7pZ7Uex_4x96lrdDd-qG9MGpCcXrpweI&_nc_ohc=yC747Fs-tb8AX_pbLmu&_nc_ht=scontent.fbau3-2.fna&oh=00_AfB2EJ9AcQzs0V9YGsY4iKFaUsMQmXf8x3ikYEPJSBtqoA&oe=64C80759" style="width:100%">
        <!--<div class="text">Caption Text</div>-->
      </div>
      <div class="mySlides fade">
        <!--<div class="numbertext">3 / 3</div>-->
        <img src="https://scontent.fbau3-1.fna.fbcdn.net/v/t31.18172-8/18491673_10213263885658241_1729706648076572917_o.jpg?_nc_cat=100&ccb=1-7&_nc_sid=cdbe9c&_nc_eui2=AeHFMLnEqMvjyVHwDlNLCFR0YVgzo2FAd7FhWDOjYUB3sRF0TKBlX2o4X4x_1ljEzlE&_nc_ohc=qhO-6Jyl3_AAX8UIeJZ&_nc_ht=scontent.fbau3-1.fna&oh=00_AfB8m2R6kWt7JvpZLh_sJ4hqjF1T8RGpxMKsn_yxUGUFCg&oe=64C80EEE" style="width:100%">
        <!--<div class="text">Caption Text</div>-->
      </div>
      <a class="prev" @click="plusSlides(-1)">❮</a>
      <a class="next" @click="plusSlides(1)">❯</a>
      </div>
      <div class="grid-container">
        <div class="grid-item-25">
        </div>
        <div class="grid-item-50">
          Teste
        </div>
        <div class="grid-item-25">
        </div>
      </div>
      <div class="grid-container">
        <div class="grid-item">
          Item 1
        </div>
        <div class="grid-item">
          Item 2
        </div>
        <div class="grid-item">
          Item 3
        </div>
        <div class="grid-item">
          Item 4
        </div>
      </div>
      <!--<div style="text-align:center">
        <span class="dot" @click="currentSlide(1)"></span> 
        <span class="dot" @click="currentSlide(2)"></span> 
        <span class="dot" @click="currentSlide(3)"></span> 
      </div>-->
    <!--<footer class="navbar">
      <img src="/src/assets/svg/footer-logo.svg" class="navbar__footer--logo" />
    </footer>-->
  </main>
</template>
<script>
import { ref, reactive, computed } from "vue";
import speedTestService from "./speedTest";
// import { useGeolocation } from "./useGeolocation.js";
import LoadingComponent from "./components/LoadingComponent.vue";
import MapsComponent from "./components/MapsComponent.vue";
import SpeedometerComponent from "./components/SpeedometerComponent.vue";
import cfDataCenters from './assets/json/cfDataCenters.json';
export default {
  components: {
    LoadingComponent,
    MapsComponent,
    SpeedometerComponent,
  },

  data () {
    return {
      slideIndex: 1
    }
  },

  setup() {
    const service = new speedTestService();
    const speedTest = reactive({
      download: "",
      upload: "",
      ping: "",
    });
    const isLoading = ref(true);
    const speedBlockLoading = ref(true);
    const locationData = reactive({
      ip: "",
      network: "",
      userLocation: "",
      serverLocation: "",
    });

    const downloadText = ref(
      "Mensura a velocidade média da sua internet durante o processo de recebimento de dados. Na maioria dos casos, a velocidade de download é a mais importante, uma vez que grande parte dos usuários recebe mais dados do que envia. É ela quem irá ditar a velocidade de carregamento de vídeos, mensagens, serviços de streaming, entre outros."
    );

    const uploadText = ref(
      "Mensura a velocidade média da sua internet durante o processo de envio de dados. A velocidade de upload é pouco utilizada pela maioria dos usuários. Ela é importante para atividades específicas, como subir um programa para a internet, fazer backup de arquivos na nuvem ou postar um vídeo no Youtube."
    );

    const pingText = ref(
      "Também conhecido como teste de latência, o primeiro dado a ser mensurado no velocímetro é o ping, importantíssimo para usuários que precisam de conexões estáveis. Essa informação ajuda a avaliar a estabilidade da sua conexão, contabilizando o tempo que leva para que um pacote de dados percorra o trajeto de ida e volta do computador de origem ao do servidor de destino."
    );

    // const { coords } = useGeolocation();

    const userPos = reactive({
      lat: undefined,
      lng: undefined
    });

    const serverPos = reactive({
      lat: undefined,
      lng: undefined
    });

    return {
      service,
      speedTest,
      isLoading,
      speedBlockLoading,
      locationData,
      userPos,
      serverPos,
      downloadText,
      uploadText,
      pingText,
    };
  },

  async mounted() {
    this.showSlides(this.slideIndex);

    this.service.addressSpeed().then((data) => {
      const serverLocation = cfDataCenters[data.colo];
      this.locationData = {
        ip: data.remoteAddress,
        network: data.asOrganization,
        userLocation: `${data.city}, ${data.region}`,
        serverLocation: serverLocation ? `${serverLocation.city}, ${serverLocation.country}` : data.colo,
      }
      this.userPos = {
        lat: +data.latitude,
        lng: +data.longitude,
      }
      this.serverPos = serverLocation ? {
        lat: serverLocation.lat,
        lng: serverLocation.lon,
      } : undefined;
    });

    await new Promise(resolve => setTimeout(resolve, 3000));
    this.isLoading = false;
    this.startSpeedTest();
  

    // setTimeout(() => {
    //   this.locationData = {
    //     ip: "192.108.0.1",
    //     network: "Ligga Telecom",
    //     userLocation: "Passo do Vigário, RS",
    //     serverLocation: "Porto Alegre, RS",
    //   };
    // }, 5000);

    // setTimeout(async () => {
    //   this.speedBlockLoading = false;
    //   this.speedTest = {
    //     download: "24.2",
    //     upload: "14.3",
    //     ping: "20.4",
    //   };
    // }, 7000);    
  },

  updated() {
    // console.log(this.userPos, this.styles);
  },

  methods: {
    restartTest() {
      this.service.abortController.abort();
      this.service.abortController = new AbortController();
      this.speedBlockLoading = true;
      this.startSpeedTest();
    },
    async startSpeedTest() {      
      this.speedBlockLoading = false;
      this.speedTest = {
        download: "",
        upload: "",
        ping: "",
      };
      this.$forceUpdate();
      await new Promise(resolve => setTimeout(resolve, 1000));
      this.speedTest.ping = await this.service.testPingSpeed();
      this.$forceUpdate();
      this.startDownloadUploadTest();
    },
    async startDownloadUploadTest() {
      await new Promise(resolve => setTimeout(resolve, 3000));
      // 100 kB
      this.speedTest.download = await this.service.testDownloadSpeed(100000);
      this.$forceUpdate();
      this.speedTest.upload = await this.service.testUploadSpeed(100000);
      this.$forceUpdate();

      await new Promise(resolve => setTimeout(resolve, 100));
      // 1 MB
      this.speedTest.download = await this.service.testDownloadSpeed(1000000);
      this.$forceUpdate();
      this.speedTest.upload = await this.service.testUploadSpeed(1000000);
      this.$forceUpdate();

      await new Promise(resolve => setTimeout(resolve, 100));
      // 10 MB
      this.speedTest.download = await this.service.testDownloadSpeed(10000000);
      this.$forceUpdate();
      this.speedTest.upload = await this.service.testUploadSpeed(10000000);
      this.$forceUpdate();

      await new Promise(resolve => setTimeout(resolve, 100));
      // 25 MB
      this.speedTest.download = await this.service.testDownloadSpeed(25000000);
      this.$forceUpdate();
      this.speedTest.upload = await this.service.testUploadSpeed(25000000);
      this.$forceUpdate();
      
      await new Promise(resolve => setTimeout(resolve, 100));
      // 100 MB
      this.speedTest.download = await this.service.testDownloadSpeed(100000000);
      this.$forceUpdate();
      
      return true;
    },
    plusSlides(n) {
      this.showSlides(this.slideIndex += n);
    },
    currentSlide(n) {
      this.showSlides(this.slideIndex = n);
    },
    showSlides(n) {
      let i;
      let slides = document.getElementsByClassName("mySlides");
      let dots = document.getElementsByClassName("dot");
      if (n > slides.length) {this.slideIndex = 1}    
      if (n < 1) {this.slideIndex = slides.length}
      for (i = 0; i < slides.length; i++) {
        slides[i].style.display = "none";  
      }
      for (i = 0; i < dots.length; i++) {
        dots[i].className = dots[i].className.replace(" active", "");
      }
      slides[this.slideIndex-1].style.display = "block";  
      dots[this.slideIndex-1].className += " active";
    }
  },
};
</script>
<style>
.v-popper--theme-tooltip .v-popper__inner {
  background: #f6ae2d !important;
  color: black !important;
  /* padding: 24px; */
  border-radius: 6px;
  /* border: 1px solid #ddd; */
  box-shadow: 0 6px 30px rgba(0, 0, 0, 0.1);
  font-size: 11px;
  max-width: 400px;
  white-space: pre-line;
}
.v-popper--theme-tooltip .v-popper__arrow-outer {
  border-color: #f6ae2d !important;
}

/* MINHAS PERSONALIZACOES */
.grid-container {
  display: flex;
  flex-wrap: wrap;
}

.grid-item {
  padding: 20px;
}

.grid-item-25 {
  flex: 0 0 25%;
}

.grid-item-50 {
  flex: 0 0 50%;
  text-align: center;
  padding: 10px;
}

/* Estilos para tamanhos de tela pequenos */
@media (max-width: 767px) {
  /* Estilos para dispositivos móveis */
  .grid-item {
    flex: 0 0 100%;
  }
}

/* Estilos para tamanhos de tela médios */
@media (min-width: 768px) and (max-width: 1023px) {
  /* Estilos para tablets e dispositivos com tela média */
  .grid-item {
    flex: 0 0 50%;
  }
}

/* Estilos para tamanhos de tela grandes */
@media (min-width: 1024px) {
  /* Estilos para desktops e dispositivos com tela grande */
  .grid-item {
    flex: 0 0 25%;
  }
}

/* A PARTIR DAQUI */
* {box-sizing: border-box}
body {
  background-color: black;
  color: white;
  font-family: Verdana, sans-serif; margin:0
}
.mySlides {display: none}
img {vertical-align: middle;}

/* Slideshow container */
.slideshow-container {
  /*max-width: 1000px;*/
  position: relative;
  margin: auto;
}

/* Next & previous buttons */
.prev, .next {
  cursor: pointer;
  position: absolute;
  top: 50%;
  width: auto;
  padding: 16px;
  margin-top: -22px;
  color: white;
  font-weight: bold;
  font-size: 18px;
  transition: 0.6s ease;
  border-radius: 0 3px 3px 0;
  user-select: none;
}

/* Position the "next button" to the right */
.next {
  right: 0;
  border-radius: 3px 0 0 3px;
}

/* On hover, add a black background color with a little bit see-through */
.prev:hover, .next:hover {
  background-color: rgba(0,0,0,0.8);
}

/* Caption text */
.text {
  color: #f2f2f2;
  font-size: 15px;
  padding: 8px 12px;
  position: absolute;
  bottom: 8px;
  width: 100%;
  text-align: center;
}

/* Number text (1/3 etc) */
.numbertext {
  color: #f2f2f2;
  font-size: 12px;
  padding: 8px 12px;
  position: absolute;
  top: 0;
}

/* The dots/bullets/indicators */
.dot {
  cursor: pointer;
  height: 15px;
  width: 15px;
  margin: 0 2px;
  background-color: #bbb;
  border-radius: 50%;
  display: inline-block;
  transition: background-color 0.6s ease;
}

.active, .dot:hover {
  background-color: #717171;
}

/* Fading animation */
.fade {
  animation-name: fade;
  animation-duration: 1.5s;
}

@keyframes fade {
  from {opacity: .4} 
  to {opacity: 1}
}

/* On smaller screens, decrease text size */
@media only screen and (max-width: 300px) {
  .prev, .next,.text {font-size: 11px}
}
</style>
