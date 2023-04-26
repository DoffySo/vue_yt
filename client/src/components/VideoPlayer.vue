<script lang="ts">
import { mergeProps } from 'vue';
import { Play, SkipNext, Pause, VolumeLow, VolumeMedium, VolumeHigh, VolumeVariantOff, Fullscreen, FullscreenExit } from 'mdue';
import format from 'format-duration';

export default {
  props: {
    src: { type: String, required: false },
    controls: { type: Boolean, required: false, default: false },
    loop: { type: Boolean, required: false, default: false },
    autoplay: { type: Boolean, required: false, default: false },
    muted: { type: Boolean, required: false, default: false },
    poster: { type: String, required: false },
    preload: { type: String, required: false, default: "auto" },
  },
  data() {
    return {
      link: "http://localhost:5173/src/videos/"+this.$route.query.v+".mp4",
      playing: false,
      videoMuted: false,
      videoVolume: 10,
      preload: true,
      fullscreen: false,
      dragging: false,
      soundDragging: false,
      controls: false,
      mouseX: 0,
    }
  },
  methods: {
    play() {
      this.$refs.player.play();
      this.setPlaying(true);
    },
    pause() {
      this.$refs.player.pause();
      this.setPlaying(false);
    },
    setPlaying(state) {
      this.playing = state;
    },
    updateVolume: function(event) {
      if (this.soundDragging) {
        let bcr = this.$refs.vol.getBoundingClientRect();
        let x = Math.min(Math.max(0, (event.clientX - bcr.left) / bcr.width), 1)
        let y = Math.min(Math.max(0, (event.clientY - bcr.top) / bcr.height), 1)

        this.$data.videoVolume = x*100;
        this.$refs.volnow.style.width = `${x*100}%`;
        this.$refs.player.volume = this.$data.videoVolume / 100;
      }
    },
    unmute() {
      this.$data.videoVolume = 30;
      this.$refs.player.volume = this.videoVolume/100;

      this.$refs.volnow.style.width = `${this.$data.videoVolume}%`
    },
    mute() {
      this.$data.videoVolume = 0;
      this.$refs.volnow.style.width = `${this.$data.videoVolume}%`
      this.$refs.player.volume = this.$data.videoVolume / 100
    },
    toggleFullscreen() {
      this.$data.fullscreen = !this.$data.fullscreen
      if (this.$data.fullscreen == true) {

          if (this.$refs.vidHolder.requestFullscreen) {this.$refs.vidHolder.requestFullscreen();}

          if (this.$refs.vidHolder.webkitEnterFullscreen) {this.$refs.vidHolder.webkitEnterFullscreen();}
          
          if (this.$refs.vidHolder.msRequestFullscreen) {this.$refs.vidHolder.msRequestFullscreen();}

          const r = this.$refs.vidHolder.getBoundingClientRect();
          this.$refs.player.style.top = `50%`;
          this.$refs.player.style.left = `50%`;
          this.$refs.player.style.transform = `translate(-50%, -50%)`;
          this.$refs.player.style.position = `absolute`;
      }
        else {
          if (document.exitFullscreen) {document.exitFullscreen();}
          if (document.webkitExitFullscreen) {document.webkitExitFullscreen();}
          if (document.msExitFullscreen) {document.msExitFullscreen();}

          this.$refs.player.style.top = ``;
          this.$refs.player.style.left = ``;
          this.$refs.player.style.transform = ``;
          this.$refs.player.style.position = ``;
      }
    },
    setTime: function (event) {
      const t = document.getElementById('vid');
      let bcr = this.$refs.alllines.getBoundingClientRect();
      let x = Math.min(Math.max(0, (event.clientX - bcr.left) / bcr.width), 1)
      let y = Math.min(Math.max(0, (event.clientY - bcr.top) / bcr.height), 1)
      x = x.toFixed(4)
      this.$refs.videoline.style.transform = `translate(0%, 0%) scaleX(${x*100}%)`
      this.$refs.circle.style.left = `${x*100}%`

      t.currentTime = document.getElementById('vid').duration.toFixed(0) / 100 * (x*100);
    },
    hoverTime: function (event) {
      const t = document.getElementById('vid');
      let bcr = this.$refs.alllines.getBoundingClientRect();
      let x = Math.min(Math.max(0, (event.clientX - bcr.left) / bcr.width), 1)
      let y = Math.min(Math.max(0, (event.clientY - bcr.top) / bcr.height), 1)
      x = x.toFixed(4)
      this.$refs.selectedtime.style.transform = `translate(0%, -50%) scaleX(${x*100}%)`
      this.$refs.timer.style.left = `${x*100}%`
      this.$refs.timer.innerHTML = `${format((t.duration.toFixed(0) / 100 * x*100)*1000)}`
    },
    setVideoTime: function(event) {
      const currentTime = event.target.currentTime.toFixed(0), allTime = event.target.duration.toFixed(0), timerLeft = document.querySelector(".timeNow"), timerRight = document.querySelector(".videolen");
    
      const perc = currentTime / allTime * 100;

      this.$refs.videoline.style.transform = `translate(0%, 0%) scaleX(${perc}%)`
      this.$refs.circle.style.left = `${perc}%`

      timerLeft.innerHTML = format(currentTime*1000);
      timerRight.innerHTML = format(allTime*1000);
    },
    testt: function(event) {
      if (this.dragging) {
        const t = document.getElementById('vid');
        let bcr = this.$refs.vidHolder.getBoundingClientRect();
        let x = Math.min(Math.max(0, (event.clientX - bcr.left) / bcr.width), 1)
        let y = Math.min(Math.max(0, (event.clientY - bcr.top) / bcr.height), 1)

        x = x.toFixed(4)
        this.$refs.videoline.style.transform = `translate(0%, 0%) scaleX(${x*100}%)`
        this.$refs.circle.style.left = `${x*100}%`

        t.currentTime = document.getElementById('vid').duration / 100 * (x*100);
        this.$refs.circle.style.transform = "translate(-50%, -50%) scale(1)";
        this.$refs.timer.style.visibility = "visible";

        this.$refs.timer.style.left = `${x*100}%`
        this.$refs.timer.innerHTML = `${format((t.duration / 100 * x*100)*1000)}`
      } else {
        this.$refs.circle.style.transform = "";
        this.$refs.timer.style.visibility = "";
      }
    },
    startDrag() {
      this.dragging = true;
    },
    stopDrag() {
      this.dragging = false;
    },
    startVolDrag() {
      this.soundDragging = true;
    },
    stopVolDrag() {
      this.soundDragging = false;
    }
  },
  components: {
    Play, SkipNext, Pause, VolumeLow, VolumeMedium, VolumeHigh, VolumeVariantOff, Fullscreen, FullscreenExit
  },
  mounted() {
    window.addEventListener('mouseup', () => {
      this.stopDrag()
      this.stopVolDrag()
    });
    window.addEventListener('mousemove', (e) => {
      if (this.dragging) {
        this.$data.mouseX = e.pageX
      }
    });

    document.addEventListener('fullscreenchange', (e) => {
      if (document.webkitFullscreenElement == null) {
        this.$refs.player.style.top = ``;
          this.$refs.player.style.left = ``;
          this.$refs.player.style.transform = ``;
          this.$refs.player.style.position = ``;
          this.$data.fullscreen = false;
      }
    })

    this.$refs.volnow.style.width = `${this.$data.videoVolume}%`
    this.$refs.player.volume = this.$data.videoVolume / 100
  }
}
</script>

<template>
<div class="wrap" v-on:load="setVideoTime">
  <div class="videoHolder" id="vidHolder" ref="vidHolder" @mousemove="testt">
    <video preload type="video/mp4" id="vid" @dblclick="toggleFullscreen()" v-on:timeupdate="setVideoTime" v-on:ended="pause" v-on:playing="play"
      :src="link"
      :muted="muted"
      :autoplay="autoplay"
      controls="false"
      :loop="loop"
      :poster="poster"
      :preload="preload"
      :playsinline="true"
      :fullscreen="false"
      controlsList="nofullscreen"
ref="player">
</video>
<div class="controls" ref="controls">
  <div class="top" id="timeline"  @mousedown="startDrag" @click="setTime" @mousemove="testt" v-on:mousemove="hoverTime" ref="alllines">
    <div class="circle" ref="circle"></div>
    <div class="videoline" ref="videoline">
    </div>
    <small class="timer" ref="timer">0:00</small>
    <div class="selectedtime" ref="selectedtime"></div>
    <div class="allvideo" ref="allvideo"></div>
  </div>
  <div class="bottom">
    <div class="start">
    <div class="togglePlayer">
        <button v-if="!playing" @click="play()"> <play></play> </button>
        <button v-else @click="pause()" > <pause></pause> </button>
      </div>
      <skip-next></skip-next>
      <div class="volume">
        <volume-variant-off class="videoVolume" v-if="videoVolume < 1" v-on:click="unmute()"></volume-variant-off>
        <volume-medium class="videoVolume" v-else-if="videoVolume < 50"  v-on:click="mute()"></volume-medium>
        <volume-high class="videoVolume" v-else type="volume-2"  v-on:click="mute()"></volume-high>
        <div id="volume" @mousemove="updateVolume" @mousedown="startVolDrag" ref="vol">
          <div id="volume--now" ref="volnow"></div>
        </div>
      </div>
      <div class="time">
        <small id="timerLeft" class="timeNow">0:00</small>
        <small class="delimiter">/</small>
        <small id="timerRight" class="videolen">0:00</small>
      </div>
  </div>
  <div class="end">
    <div class="fullscreen">
      <fullscreen class="videoVolume" v-if="fullscreen == false"  v-on:click="toggleFullscreen()"></fullscreen>
        <fullscreen-exit class="videoVolume" v-else v-on:click="toggleFullscreen()"></fullscreen-exit>
      </div>
  </div>
  </div>
</div>
  </div>
</div>
</template>

<style lang="scss">

.t {
  position: absolute;
  top: 0;
  left: 0;
}

.wrap {
  display: flex;
  flex-direction: column;
  position: relative;
  align-items: center;
  justify-content: center;
  width: 60%;
  height: auto;
  .videoHolder {
    display: flex;
    flex-direction: column;
    position: relative;
  }

  &:hover {
    .controls {
      display: flex;
    }
  }
  video {
    width: 100%;
    &::-webkit-media-controls-enclosure {
      display:none !important;
    }
  }

  .top {
    display: flex;
    width: 100%;
    height: 4px;
    cursor: pointer;
    justify-content: center;
    text-align: center;
    align-items: center;
    position: relative;

    &:hover {
      .circle {
        transform: translate(-50%, -50%) scale(1);
      }
      .timer {
        visibility: visible;
      }
      height: 6px;
      transform: translateY(-.5px);
    }

    &:active {
      cursor: pointer;
    }

    .circle {
      content: '';
      min-width: 16px;
      min-height: 16px;
      border-radius: 32px;
      background-color: red;
      position: absolute;
      transform: translate(-50%, -50%) scale(0);
      left: 0%;
      top: 50%;
      transition: transform 100ms ease;
      z-index: 2021;
    }
    .videoline {
      width: 100%;
      display: flex;
      height: 100%;
      background-color: red;
      z-index: 2020;
      transform: translate(0%, -50%) scaleX(0%);
      left: 0%;
      top: 50%;
      transform-origin: 0% 100%;
    }
    .selectedtime {
      width: 100%;
      margin: 0px;
      height: 100%;
      background-color: rgb(189, 189, 189);
      position: absolute;
      transform: translate(0%, -50%) scaleX(0);
      top: 50%;
      left: 0;
      z-index: 2018;
      transform-origin: 0% 100%;
    }
    .timer {
      color: white;
      text-align: center;
      align-items: center;
      position: absolute;
      transform: translate(-50%, -200%);
      font-weight: 500;
      text-shadow: 0 1px 5px black;
      font-family: 'Roboto', sans-serif;
      top: 100%;
      left: 0%;
      width: max-content;
      padding: 2px 6px;
      font-size: 12px;
      border-radius: 3px 3px 3px 3px;
      background-color: none;
      visibility: hidden;
    }
    .allvideo {
      width: 100%;
      height: 100%;
      background-color: rgb(153, 153, 153);
      position: absolute;
      transform: translate(0%, -50%);
      top: 50%;
      left: 0;
      z-index: 2015;
      max-width: 100%;
    }
  }
  .bottom {
    display: flex;
    width: 100%;
    align-items: center;
    text-align: center;
    justify-content: space-between;
  }
  .controls {
  z-index: 2147483649;
  display: flex;
  align-self: flex-end;
  flex-direction: column;
  width: 100%;
  height: 50px;
  padding: 0 10px;
  text-align: center;
  justify-content: space-between;
  align-items: center;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 7px;

  svg {
      font-size: 32px;
      transition: 300ms ease;
      color: white;
      cursor: pointer;
    }
    .start {
      display: flex;
      align-items: center;
      .togglePlayer{
        display: flex;
        width: 32px;
        height: 32px;
        overflow: hidden;
        justify-content: center;
        align-items: center;
        text-align: center;
        margin: 0 6px;
        button {
          width: 32px;
          height: 32px;
          color: white;
          background: transparent;
          font-size: 32px;
          border: 0;
          cursor: pointer;
      }
      }
      .volume {
        align-items: center;
        text-align: center;
        justify-content: left;
        display: flex;
        height: 40px;
        min-width: 40px;
        margin: 0 6px;
        &:hover {
          width: max-content;
          #volume {
            width: 56px;
            visibility: visible;
            transition: all 150ms ease;
            transition-delay: 100ms;
            &--now {
              &::before {
                display: flex;
                width: 12px;
                height: 12px;
                opacity: 1;
              }
            }
          }
        }
        .videoVolume {
          color: white;
          margin-left: 6px;
          margin-right: 6px;
        }
        #volume {
          background: rgba(255,255,255,.2);
          width: 0px;
          visibility: hidden;
          transition: all 200ms ease;
          transition-duration: 200ms;
          height: 4px;
          display: flex;
          cursor: pointer;
          &--now {
            width: 0%;
            background-color: white;
            position: relative;
            &::before {
              position: absolute;
              left: 100%;
              top: 50%;
              transform: translate(-50%, -50%);
              display: flex;
              width: 12px;
              height: 12px;
              opacity: 0;
              border-radius: 24px;
              background: #fff;
              content: '';
            }
          }
        }
      }
      .time {
        color: white;
        font-size: 16px;
        margin-left: 10px;
        align-items: center;
        text-align: center;

        .delimiter {
          margin-left: 5px;
          margin-right: 5px;
        }
      }
    }
    .end {
      .fullscreen {
        display: flex;
        margin: 0 6px;
      }
    }
}
}
</style>