<template>
<div class="bar">
<div class="mdui-appbar mdui-appbar-fixed">
  <div class="mdui-toolbar mdui-color-indigo">
    <a href="javascript:;" @click="menu" class="mdui-btn mdui-btn-icon"><i class="mdui-icon material-icons">menu</i></a>
    <a href="javascript:;" class="mdui-typo-headline">JDC</a>
    <a href="javascript:;" class="mdui-typo-title">{{title}}</a>
    <div class="mdui-toolbar-spacer"></div>
    <a href="javascript:;" class="mdui-btn mdui-btn-icon" mdui-dialog="{target: '#assgnment'}"><i class="mdui-icon material-icons">assignment</i></a>
  </div>
</div>
<div class="mdui-dialog" id="assgnment">
    <div class="mdui-dialog-content">{{notice}}</div>
    <div class="mdui-dialog-actions">
      <button class="mdui-btn mdui-ripple" mdui-dialog-close>了解</button>
    </div>
  </div>
</div>
</template>

<script>
import mdui from 'mdui'
import axios from 'axios'
import global from '../components/Common.vue'
export default {
    name:'AppBar',
    data:function(){
      return {
        baseUrl:"",
        notice:"",
      }
    },
    methods:{
      menu(){
          var inst = new mdui.Drawer('#drawer');
          inst.toggle();
      },
    },
    mounted:function(){
      var self= this
      if(localStorage.getItem("baseUrl")!=""&&localStorage.getItem("baseUrl")!=null){
        this.baseUrl=localStorage.getItem("baseUrl")
      }else{
        this.baseUrl=global.urlList[0].url
      }
      axios.get(this.baseUrl+"/notice").then(function(res){
        self.notice=res.data.data
      })
    },
    props:{
      title: String
    }
}
</script>

<style scoped>
</style>