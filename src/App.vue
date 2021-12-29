<template>
  <div id="app" class="m-1">
      <b-tabs pills content-class="tab-content" fill>
        <b-card class="m-1">
          <b-card-title id="title">タップまたはマウスホバーで詳細情報が見れます。</b-card-title>
          <b-card-text id="description"></b-card-text>
        </b-card>
        <b-tab :title="i.name" v-for="(i,j) in content" :key="i.id"><tree :content="i" :id="j"></tree></b-tab>
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
      content: {}
    }
  },
  beforeMount: function (){
    axios.get("https://data.arainu.world/api/advancements/")
    .then(response => {
      console.log(response.data)
      this.$data.content = response.data
    })
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
