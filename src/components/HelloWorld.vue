<template>
  <div>
    <h2>中原小斑鸠</h2>
    <div class="box">
      <section class="left">
        <div id="videolay">
          <!-- <video id="video" :width="700" controls autoplay="true" class="video">
        　　<source style="width:700px;" src = "http://ivi.bupt.edu.cn/hls/cctv1hd.m3u8"/>
      　　</video> -->
        </div>
      </section>
      <section class="right">
        <div class="channelBox">
          <div class="btn">
            <button
              :class="selectInd == index ? 'selected' : 'noSelected'"
              @click="selectChannel(item, index)"
              v-for="(item, index) in list"
              :key="index"
            >
              {{ item.name }}
            </button>
          </div>
        </div>
      </section>
    </div>
    <div class="footer">注：因网络原因部分频道可能会等待超时、播放出错。</div>
  </div>
</template>

<script>
import back from "../assets/back.jpg";
import { channel } from "./channel.js";
console.log(channel);
export default {
  name: "HelloWorld",
  data() {
    return {
      list: channel,
      selectInd:0,
      dom:null
    };
  },
  mounted() {
    this.dom=document.getElementById('videolay')
    console.log(window.videojs)
    if(localStorage.getItem('url')){
      this.getDom(localStorage.getItem('url'))
    }else{
      this.getDom('http://ivi.bupt.edu.cn/hls/cctv1hd.m3u8')
    }
    if(!localStorage.getItem('index') && localStorage.getItem('index')!=0){
      this.selectInd=0
    }else{
      this.selectInd=localStorage.getItem('index')
    }
  },
  methods: {
    getDom(url){
      var ovideo=document.createElement('video')
      ovideo.setAttribute('id','video')
      ovideo.setAttribute('width','700')
      ovideo.setAttribute('autoplay','true')
      var osource=document.createElement('source')
      osource.setAttribute('src',url)
      osource.setAttribute('style','width:700px;')
      ovideo.appendChild(osource)
      this.dom.appendChild(ovideo)
      this.$nextTick(()=>{
        document.getElementById('video_html5_api').style.width='700px'
        document.getElementById('video_html5_api').style.height='500px'
      })
      this.player = window.videojs('video',{
        muted: true,
        controls : false,   
        width:700, 
        // 更多配置.....
      },()=>{
        console.log('准备考了')
      }); 
      //播放
      this.player.play(); 
    },
    selectChannel(item, index) {
      // document.getElementById('video_html5_api').src=this.list[index].url
      // this.dom.removeChild()
      // console.log(this.dom)
      // document.getElementById('video').load()
      localStorage.setItem('url',this.list[index].url)
      localStorage.setItem('index',index)
      console.log(localStorage.getItem('url'))
      window.location.reload()
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h2 {
  text-align: center;
  height: 100px;
  line-height: 100px;
  color: yellow;
  margin-bottom: 30px;
}
.box {
  display: flex;
  justify-content: space-between;
}
.left {
  width: 60%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.right {
  width: 40%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.video {
  width: 100%;
  height: 100%;
}
#videolay {
  width: 700px;
  height: 500px;
  position: relative;
}
#video_html5_api{
  width:700px;
  height: 500px;
}
.layer {
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.8);
  position: absolute;
  z-index: 100;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.nolayer {
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0);
  position: absolute;
  z-index: 100;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.channelBox {
  overflow: hidden;
}
.btn {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  overflow: auto;
  height: 500px;
}
.btn button {
  width: 110px;
  height: 30px;
  font-size: 12px;
  margin: 10px 20px;
  cursor: pointer;
}
.footer {
  height: 100px;
  line-height: 100px;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  color: yellow;
  font-size: 24px;
  text-align: center;
}
.noSelected {
  background: #fff;
  color: #000;
}
.selected {
  background: yellow;
  color: #000;
}
</style>
