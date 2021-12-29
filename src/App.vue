<template>
  <div id="app" class="m-1">
      <b-tabs pills content-class="tab-content">
        <b-card>
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
    axios.get("https://asia-northeast1-swift-arcadia-332108.cloudfunctions.net/get_advancements")
    .then(response => {
      console.log(response.data)
      this.$data.content = response.data
    })
  }
}
</script>

<style>
</style>
