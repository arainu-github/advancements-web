<template>
  <div id="app" class="m-1">
    <div id="loading">
      <div class="spinner">
        <div class="cube1"></div>
        <div class="cube2"></div>
      </div>
      <p class="text-center m-0" style="font-size: 20px;">進捗データの読み込み中...</p>
      <p class="text-center">Create by Kumitatepazuru</p>
    </div>
    <div id="content">
      <b-tabs pills content-class="tab-content" fill>
        <b-card class="m-1">
          <b-card-title id="title">タップまたはマウスホバーで詳細情報が見れます。</b-card-title>
          <b-card-text id="description"><br></b-card-text>
          <b-progress :max="max" ariant="info" :striped="true" height="2rem">
            <b-progress-bar :value="percent" :label="`${percent}/${max}`"></b-progress-bar>
          </b-progress>
        </b-card>
        <b-tab :title="i.name" v-for="(i,j) in content" :key="i.id"><tree :content="i" :id="j" :player="player" @updateProgress="updateProgress"></tree></b-tab>
      </b-tabs>
      </div>
  </div>
</template>

<script>
import Tree from "@/components/tree";
import axios from "axios";
export default {
  name: 'App',
  components: {Tree},
  data() {
    return {
      content: [],
      player: [],
      percent: 0,
      max: 0
    }
  },
  mounted: function (){
    const urlParams = new URLSearchParams(window.location.search);
    const uuid = urlParams.get('uuid');
    axios.get("https://data.arainu.world/api/advancements/")
    .then(response => {
      this.$data.content = response.data
      if(uuid === null){
        document.getElementById("loading").remove();
      }
    })
    if(uuid !== null) {
      axios.get("https://data.arainu.world/api/advancements/player?uuid=" + uuid)
          .then(response => {
            this.$data.player = response.data
            document.getElementById("loading").remove();
          })
    }
  },
  methods: {
    updateProgress(percent,max){
      this.percent = percent;
      this.max = max;
      console.log(percent,max)
    }
  }
}
</script>

<style>
.nav {
  flex-direction: column;
  padding-bottom: 5px;
  max-height: 70px;
  overflow-x: auto;
}

.spinner {
  margin: 100px auto;
  width: 40px;
  height: 40px;
  position: relative;
}

.cube1, .cube2 {
  background-color: #333;
  width: 15px;
  height: 15px;
  position: absolute;
  top: 0;
  left: 0;

  -webkit-animation: sk-cubemove 1.8s infinite ease-in-out;
  animation: sk-cubemove 1.8s infinite ease-in-out;
}

.cube2 {
  -webkit-animation-delay: -0.9s;
  animation-delay: -0.9s;
}

@-webkit-keyframes sk-cubemove {
  25% { -webkit-transform: translateX(42px) rotate(-90deg) scale(0.5) }
  50% { -webkit-transform: translateX(42px) translateY(42px) rotate(-180deg) }
  75% { -webkit-transform: translateX(0px) translateY(42px) rotate(-270deg) scale(0.5) }
  100% { -webkit-transform: rotate(-360deg) }
}

@keyframes sk-cubemove {
  25% {
    transform: translateX(42px) rotate(-90deg) scale(0.5);
    -webkit-transform: translateX(42px) rotate(-90deg) scale(0.5);
  } 50% {
      transform: translateX(42px) translateY(42px) rotate(-179deg);
      -webkit-transform: translateX(42px) translateY(42px) rotate(-179deg);
    } 50.1% {
        transform: translateX(42px) translateY(42px) rotate(-180deg);
        -webkit-transform: translateX(42px) translateY(42px) rotate(-180deg);
      } 75% {
          transform: translateX(0px) translateY(42px) rotate(-270deg) scale(0.5);
          -webkit-transform: translateX(0px) translateY(42px) rotate(-270deg) scale(0.5);
        } 100% {
            transform: rotate(-360deg);
            -webkit-transform: rotate(-360deg);
          }
}

#loading {
  width: 100vw;
  height: 100vh;
  background-color: #0bd;

  /* 以下のコードを追加 */
  position: fixed;
  top: 0;
  left: 0;
  z-index: 9999;
}
</style>
