<template>

  <div id="app" :style="{ backgroundImage:'url(../static/bg/' + bgSet[bgIndex_fin] + ')'}">
    <div id="setting" onmouseout="style.opacity='0.5'" onmouseover="style.opacity='1'">
      <i class="fa fa-cog fa-2x" aria-hidden="true" @click="setBgBox()"></i>
      <div id="bgBox" v-show="bgBox_show">
        <div id="changeBg" @click="bgBox_show=1">
          
          <div style="height:20px"></div>         
          
          <div id="last" onmouseout="style.color='white'" onmouseover="style.color='skyblue'">
            <i class="fa fa-chevron-left fa-2x" aria-hidden="true" @click="changeBgFunc(-1)"></i>
          </div>

          <div id="tempBg">  
            <img :src="require('../static/bg/' + bgSet[bgIndex_temp])" width="120px" height="120px">           
          </div>

          <div id="next" onmouseout="style.color='white'" onmouseover="style.color='skyblue'">
            <i class="fa fa-chevron-right fa-2x" aria-hidden="true" @click="changeBgFunc(1)"></i>
          </div>      

          <div id="confirm" onmouseout="style.color='white'" onmouseover="style.color='skyblue'">
            <i class="fa fa-check-square fa-2x" aria-hidden="true" @click="confirmBg()"></i>
          </div>

        </div>
      </div>  
    </div>
    
    
    <router-view v-wechat-title='$route.meta.title'></router-view>  <!--加入该句，可以修改网页的标题-->
  </div>

</template>

<script>
export default {
  name: 'App',
  data(){
    return {
      bgIndex_fin:0,
      bgIndex_temp:0,
      bgBox_show:0,
      bgSet:[
        'bg0.jpg',
        'bg1.jpg',
        'bg2.jpg',
        'bg3.jpg',
        'bg4.jpg',
        'bg5.jpg',
        'bg6.jpg',
        'bg7.jpg',
        'bg8.jpg',
        'bg9.jpg',
      ]
    }
  },

  mounted: function(){
    this.bgIndex_fin = Math.floor(Math.random() * (this.bgSet.length+1));
    console.log("random = ", this.bgIndex_fin);
  },

  methods:{
    
    // 背景设置开关
    setBgBox(){
      this.bgBox_show=!this.bgBox_show;
    },

    // 修改背景
    changeBgFunc(op){
      if(op==1){
        this.bgIndex_temp++;
        if(this.bgIndex_temp>=this.bgSet.length){
          this.bgIndex_temp=0;
        }
      }
      else{
        this.bgIndex_temp--;
        if(this.bgIndex_temp<=0){
          this.bgIndex_temp=this.bgSet.length-1; 
        }
      }
    },

    // 确认修改背景
    confirmBg(){
      this.bgIndex_fin=this.bgIndex_temp;
    }
  },  
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  padding:0px;
  margin:0px;
  position:absolute;
  top:0px;
  left:0px;
  width:100%;
  height:100%;
  border:hidden;

  position: fixed;
  background-size: 100% 100%;
}

#setting {
  
  position: fixed;
  margin-top: 50px;
  width: 300px;
  height: 300px;
  opacity: 0.5;
  
}

#bgBox{
  width:300px;
  height: 200px;
  
}

#changeBg{
  width: 200px;
  height: 200px;
  background: #323232;
  margin-left: 50px;
}

#last{
  color: white;
  margin-top: 40px;
  float: left;
  width: 40px;
  height: 40px;
}

#next{
  color: white;
  margin-top: 40px;
  float: left;
  width: 40px;
  height: 40px;
}

#tempBg{
  float: left;
  width:120px;
  height: 120px;
}

#confirm{
  color:white;
  float: left;
  margin-top: 20px;
  width:200px;
  text-align: center;
}

</style>
