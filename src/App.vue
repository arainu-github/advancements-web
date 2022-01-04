<template>
  <div id="app" class="m-1">
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
  beforeMount: function (){
    axios.get("https://data.arainu.world/api/advancements/")
    .then(response => {
      this.$data.content = response.data
    })
    const urlParams = new URLSearchParams(window.location.search);
    const uuid = urlParams.get('uuid');
    if(uuid !== null){
      axios.get("https://data.arainu.world/api/advancements/player?uuid="+uuid)
          .then(response => {
            this.$data.player = response.data
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
</style>
