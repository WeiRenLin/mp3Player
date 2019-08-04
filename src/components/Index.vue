<template>
  <div class="index">
    <div class="menu">
<ul>
  <li>Browse</li>
  <li>Radio</li>
  <li></li>
   <li>YOUR LIBRARY</li>
  <li>Recently Played</li>
  <li>Songs</li>
  <li>Albums</li>
  <li>Artists</li>
</ul>
    </div>
    <div class="right">
 <div class="up__text">
   <div class="up__text__bar">

   </div>
   <div class="up__text__song__detail">
   <img src="../../public/static/img/withoutYou.jpg" alt="">
  <div class="" >
    <ul>
      <li>Artists</li>
      <li></li>
      <li></li>

    </ul>
  </div>
   </div>

    </div>
    <div class="bottom__song">
      <div class="bottom__song__overview">
        <h3>OverView</h3>
        <h4>About</h4>
        <article>Maroon 5 is an American pop rock band from Los Angeles, California. It currently consists of lead vocalist Adam Levine, keyboardist 
          and rhythm guitarist Jesse Carmichael, bassist Mickey Madden, lead guitarist James Valentine, drummer Matt Flynn, keyboardist PJ Morton, 
          and multi-instrumentalist Sam Farrar. Original members Levine, Carmichael, Madden, and drummer Ryan Dusick first came together as Kara’s Flowers 
          in 1994, while they were still in high school. After self-releasing their independent album We Like Digging?, the band signed to Reprise Records 
          and released the album The Fourth World in 1997. The album garnered a tepid response, 
          after which the record label dropped the band and the members focused on college.</article>
      </div>
      <div class="bottom__song__list">
        <div class="bottom__song__list__menu">
          <ul>
          <li>SONGS</li>
          <li>ALBUMS</li>
          <li>CONCERTS</li>
          <li>NEWS</li>
        </ul>
        </div>
         <div class="bottom__song__list__item">
           <h4>POPULAR RELEASES</h4>
           <table>
             <tr v-for="(item,index) in songList" :index="index" >
               <td class="number">{{item.id}}</td>
               <td class="plus">+</td>
               <td class="song__name">{{item.title}}</td>
               <td class="time"></td>
             </tr>
              
           </table>
         </div>
      </div>
    </div>
    </div>
   <div class="bottom__progress" refs="progress" id="progress">
     <!-- <input type="range" style="padding: 0; margin: 0;width: 100%;" 
			 min="0" max="100" @change="changeCurrentTime"> -->
   </div>
    <div class="bottom__mp3">

<audio ref="audio" @canplay="getDuration"  :current-time.prop="durationTime" @timeupdate="onTimeupdate" preload="auto" playbackRate=1 >
 </audio>
 <div class="inner">
   <div class="narrative">
   <img src="../../public/static/img/withoutYou.jpg" alt="">
  </div>
  <div class="durationTime">
<h5>我真的好想你</h5>
<div class="time"></div>
 {{minTime+":"+secTime}}/{{totalTime}}
   </div>
 <div class="main">
    <button  @click="pause"><img src="../../public/static/img/skipBack.svg" alt=""></button>
    <button  v-show="paused" @click="play"><img src="../../public/static/img/play.svg" alt=""></button>
    <button v-show="playing" @click="pause"><img src="../../public/static/img/pause.svg" alt=""></button>
    <button  @click="pause"><img src="../../public/static/img/skipForward.svg" alt=""></button>
 </div>
 <div class="volume">
   <div v-if="hasMuted == false">
     <button @click="onMuted">
   <img src="../../public/static/img/volumeUp.svg" alt="">
   </button>
   </div>
   <div v-if="hasMuted == true">
     <button @click="onVolume">
   <img src="../../public/static/img/volumeMute.svg" alt="">
   </button>
   </div>
   <div class="volumeProgress" refs="volumeProgress" id="volumeProgress">
     <input type="range" style="padding: 0; margin: 0;width: 100%;" 
			 min="0" max="100" @change="changeVolume">
   </div>
 </div>
 </div>
 
    </div>
  </div>
</template>

<script>
//@pause="onPause"
    // @play="onPlay"
    // @timeupdate="onTimeupdate"
export default {
  name: 'Index',
  data(){
return{
    paused:true,
    durationTime:0,
     minTime: '00',
      secTime: '00',
      totalTime:"00:00",
      totalAllTime:0,
      progress:null,
      hasMuted:false,
      hasLoud:false,
      songList:[
          {
            id:1,title:"Without You",src:require("@/assets/audio/withoutYou.mp3")
          },
          {
            id:2,title:"where Is My Ex",src:require("@/assets/audio/whereIsMyEx.mp3")
          },
           {
            id:3,title:"Love Song For You",src:require("@/assets/audio/loveSongForYou.mp3")
          },
           {
            id:4,title:"Evaporation",src:require("@/assets/audio/evaporation.mp3")
          },
           {
            id:5,title:"Courage",src:require("@/assets/audio/courage.mp3")
          }
        ],
}
  },
  methods:{
    play:function(){
      this.$refs.audio.play()
      this.paused = false;
    },
    pause:function(){
      this.$refs.audio.pause()
      this.paused = true;
    },
    getDuration:function(e){
      var vm  = this;
      var duration = e.target.duration;
      vm.totalAllTime = duration;
      var minTime = (Math.floor(duration / 60))>10?(Math.floor(duration / 60)).toString():"0"+(Math.floor(duration / 60)).toString();
      var secTime = duration % 60 > 10 ? (Math.floor((duration % 60))).toString() : "0" + (Math.floor((duration % 60))).toString();
      vm.totalTime = minTime + ":" + secTime;
    },
    onMuted:function(){
      this.hasMuted = true;
      this.$refs.audio.muted = !this.$refs.audio.muted
    },
    onVolume:function(){
      this.hasMuted = false;
      this.$refs.audio.muted = !this.$refs.audio.muted;
      this.$refs.audio.volume = this.$refs.audio.volume;
    },
    changeVolume:function(e){
      this.$refs.audio.volume =e.target.value/100
    },
// 當timeupdate事件大概每秒一次，用來更新音樂的當前播放時間
    onTimeupdate(val) {
      var vm  = this;
      var currentTime = val.target.currentTime;
        vm.$refs.audio.durationTime = currentTime;
         var progress = document.getElementById('progress');
      vm.minTime = (Math.floor(currentTime / 60))>10?(Math.floor(currentTime / 60)).toString():"0"+(Math.floor(currentTime / 60)).toString();
      vm.secTime =currentTime % 60 > 10 ? (Math.floor((currentTime % 60))).toString() : "0" + (Math.floor((currentTime % 60))).toString();
      var stopTime = currentTime;
      var totalTime = vm.totalAllTime;
      progress.style.width = stopTime / totalTime * 100+ '%';
      },
    // changeCurrentTime:function(){
    //   alert()
    // }
  },
  computed: {
    playing() { return !this.paused; }
  },
  watch:{
    durationTime:function(val){
     
    }
  },
  mounted(){
    // this.$refs.audio.src = this.songList[0].src
    this.$refs.audio.setAttribute("src", this.songList[0].src);

  }
}
</script>

