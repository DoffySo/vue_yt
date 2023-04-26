<!-- <script setup lang="ts">
defineProps<{
    title?: string
    views?: number
    avatarUrl?: string
    thumbnailUrl?: string
    author?: string
    timeago?: string
    time?: string
    linkToVid?: string
}>()
</script> -->

<script lang="ts">
import format from 'format-duration';

import { VolumeMedium, VolumeOff, ClosedCaptionOutline, ClosedCaption } from 'mdue';

export default {
  data() {
    return {
      link: `http://localhost:5173/src/videos/${this.linkToVid}.mp4`,
      redirectLink: `watch?v=${this.$props.linkToVid}&channel=${this.$props.author}`,
      muted: true,
      cc: false,
      dragging: false,
    }
  },
  components: {
    VolumeMedium, VolumeOff, ClosedCaptionOutline, ClosedCaption
  },
  props: ['title', 'views', 'avatarUrl', 'thumbnailUrl', 'author', 'timeago', 'time', 'linkToVid'],
  methods: {
    redirect: function(link: String, channel: String) {
      window.location.href = `watch?v=${this.$props.linkToVid}&channel=${this.$props.author}`;
    },
    updateTimer: function(event) {
      const plr = event.target;

      const percent = plr.currentTime / plr.duration * 100;

      this.$refs.timer.innerHTML = `${format(plr.currentTime*1000)} / ${format(plr.duration*1000)}`
      this.$refs.timeline.style.width = `${percent}%`
    },
    startPlay: function(event) {
      this.$refs.vid.play()
    },
    stopPlay: function(event) {
      this.$refs.vid.pause()
    },
    toggleSound: function(event) {
      this.$data.muted = !this.$data.muted;

      this.$data.muted == true ? this.$refs.vid.volume = 0 : this.$refs.vid.volume = 0.1;
    },
    hoverTime: function(event) {
      let bcr = this.$refs.alltime.getBoundingClientRect();
      let x = Math.min(Math.max(0, (event.clientX - bcr.left) / bcr.width), 1)
      let y = Math.min(Math.max(0, (event.clientY - bcr.top) / bcr.height), 1)

      this.$refs.timer.style.opacity = 0
      this.$refs.selectiontimer.style.opacity = 1

      this.$refs.selectiontimer.style.left = `${x*100 - 3}%`
      const time = this.$refs.vid.duration.toFixed(0) / 100 * (x*100)
      this.$refs.selectiontimer.innerHTML = format(time*1000)
    },
    unhoverTime: function(event) {
      this.$refs.timer.style.opacity = 1
      this.$refs.selectiontimer.style.opacity = 0
    },
    setTime: function(event) {
      let bcr = this.$refs.alltime.getBoundingClientRect();
      let x = Math.min(Math.max(0, (event.clientX - bcr.left) / bcr.width), 1)
      let y = Math.min(Math.max(0, (event.clientY - bcr.top) / bcr.height), 1)

      const time = this.$refs.vid.duration.toFixed(0) / 100 * (x*100)

      this.$refs.vid.currentTime = time;
    },
    startDrag() {
      this.dragging = true;
    },
    stopDrag() {
      this.dragging = false;
    },
    setTimeDragging: function(event) {
      this.hoverTime(event)
      if (this.dragging == true) {
        let bcr = this.$refs.alltime.getBoundingClientRect();
        let x = Math.min(Math.max(0, (event.clientX - bcr.left) / bcr.width), 1)
        let y = Math.min(Math.max(0, (event.clientY - bcr.top) / bcr.height), 1)

        x = x.toFixed(4)
        const time = this.$refs.vid.duration.toFixed(0) / 100 * (x*100)
        this.$refs.vid.currentTime = time;
      }
    },
  }
}
</script>
<template>
<div class="video">
<div class="content">
<div class="video-holder">
    <div class="dismissible">
    <div class="thumbnail" @mouseenter="startPlay" @mouseleave="stopPlay">
        <div class="miniPlayer">
          <video ref="vid" volume="0" :src="link" @timeupdate="updateTimer" @click="redirect"></video>
          <div class="controls">
            <div class="topcontrols">
              <div class="sound" @click="toggleSound">
                <volume-medium v-if="muted == false"></volume-medium>
                <volume-off v-else></volume-off>
              </div>
              <div class="cc">
                <closed-caption-outline v-if="cc == false"></closed-caption-outline>
                <closed-caption v-else></closed-caption>
              </div>
            </div>
            <small ref="timer" style="opacity: 1" class="timer"></small>
            <small ref="selectiontimer" style="opacity: 0" class="selectionTimer">1:11</small>
            <div class="timeline" style="width: 0%;" ref="timeline">
              <div class="circle"></div>
            </div>
            <div class="alltime" ref="alltime" @mousedown="startDrag" @mouseup="stopDrag" @mousemove="setTimeDragging" @mouseleave="unhoverTime" @click="setTime"></div>
            <div class="bgtimeline"></div>
          </div>
        </div>
        <RouterLink :to=redirectLink>
        <img :src="$props.thumbnailUrl" alt="">
        </RouterLink>
        <div class="details">
        <div class="time-status">
            <span>{{ time }}</span>
        </div>
        </div>
    </div>
    <div class="details">
        <RouterLink class="user-image" :to=redirectLink>
        <img :src="$props.avatarUrl" alt="">
        </RouterLink>
        <div class="meta">
        <h3 class="text-primary">
            <RouterLink :to=redirectLink>
            <span>{{ title }}</span>
            </RouterLink>
        </h3>
        <div class="metadata">
            <div class="byline-container">
            <span>{{ author }}</span>
            </div>
            <div class="metadata-line">
            <span class="views">{{ views }} views</span>
            <span class="separator">•</span>
            <span class="time">{{ timeago }}</span>
            </div>
        </div>
        </div>
    </div>
    </div>
</div>
</div>
</div>
</template>


<style lang="scss">
$--ytd-rich-grid-items-per-row: 3;
$--ytd-rich-grid-item-max-width: 360px;
$--ytd-rich-grid-item-margin: 16px;
$--yt-button-compact-background-color: 0;
$--yt-button-compact-text-color: 0;
$--yt-spec-text-primary: #f1f1f1;
$--yt-spec-text-secondary: #aaa;


.video {
          position: relative;
          margin-left: calc($--ytd-rich-grid-item-margin/2);
          margin-right: calc($--ytd-rich-grid-item-margin/2);
          margin-bottom: 40px;
          width: calc(100%/$--ytd-rich-grid-items-per-row - $--ytd-rich-grid-item-margin - 0.01px);

          .content {
            height: 100%;
            display: flexbox;
            display: flex;
            justify-content: center;

            .video-holder {
              //background-color: $--yt-spec-10-percent-layer;
              color: $--yt-spec-text-secondary;
              width: 100%;
              margin: 0;
              display: block;
              max-width: $--ytd-rich-grid-item-max-width;
              position: relative;
              font-size: 14px;

              .dismissible {
                position: relative;
                height: 100%;
                display: flexbox;
                display: flex;
                flex-direction: column;

                &:hover {
                  cursor: pointer;
                  .thumbnail > a > img {
                  border-radius: 0;
                  transition: border-radius 350ms ease-in;
                  }
                  .thumbnail > .details > .time-status {
                    opacity: 0;
                    transition: 350ms ease;
                  }
                }
                .thumbnail {
                  width: 100%;
                  position: relative;

                  &:hover {
                    .miniPlayer {
                      opacity: 1;
                      transition: 200ms ease;
                      transition-delay: 400ms;
                    }
                  }

                  .miniPlayer {
                    display: flex;
                    opacity: 0;
                    bottom: 0; 
                    right: 0; 
                    left: 50%; 
                    top: 0; 
                    width: 100%; 
                    height: 100%;
                    position: absolute; 
                    transform: translate(-50%, 0%);
                    z-index: 20;
                    padding-bottom: 3px;

                    video {
                      width: 100%;
                      height: 100%;
                    }

                    .controls {
                      width: 100%;
                      height: 4px;
                      z-index: 25;
                      position: absolute;
                      bottom: 0;
                      transition: 200ms ease;

                      .topcontrols {
                        width: 80px;
                        height: 32px;
                        background: #00000060;
                        border-radius: 4px;
                        position: absolute;
                        right: 10px;
                        bottom: 160px;
                        &:hover {
                          background: #000000dd;
                        }
                        color: white;
                        display: flex;
                        align-items: center;
                        justify-content: space-evenly;

                        .sound {
                          height: 100%;
                          width: 50%;
                          font-size: 24px;
                          display: flex;
                          align-items: center;
                          justify-content: center;
                        }
                        .cc {
                          height: 100%;
                          width: 50%;
                          font-size: 24px;
                          display: flex;
                          align-items: center;
                          justify-content: center;
                        }
                      }

                      .timer {
                        position: absolute;
                        left: 10px;
                        top: -20px;
                        transform: translate(0, 0);
                        color: white;
                        font-weight: 500;
                        text-shadow: 0 2px 2px black;
                      }
                      .selectionTimer {
                        color: white;
                        position: absolute;
                        bottom: 10px;
                        transform: translate(0, -50%);
                        left: 50%;
                        text-shadow: 0 2px 2px black;
                      }

                      &:hover {
                        .timeline {
                          .circle {
                            transform: translate(-50%, -50%) scale(1);
                          }
                        }
                      }

                      .alltime {
                        width: 100%;
                        height: 100%;
                        position: absolute;
                        bottom: 0px;
                        z-index: 20;
                      }
                      .bgtimeline {
                        width: 100%;
                        height: 100%;
                        position: absolute;
                        bottom: 0px;
                        background: #aaa;
                        z-index: 18;
                      }
                      .timeline {
                        background: red;
                        height: 100%;
                        position: absolute;
                        bottom: 0px;
                        z-index: 19;
                        

                        .circle {
                          width: 16px;
                          height: 16px;
                          background: red;
                          transform: translate(-50%, -50%) scale(0);
                          top: 50%;
                          left: 100%;
                          display: flex;
                          position: relative;
                          border-radius: 32px;
                          transition: transform 200ms ease;
                        }
                      }
                    }
                  }

                  a {
                    position: relative;
                    top: 0;
                    right: 0;
                    bottom: 0;
                    left: 0;
                    border-radius: 12px;
                    height: 100%;
                    margin-left: auto;
                    margin-right: auto;
                    img {
                      object-fit: cover;
                      border-radius: 16px;
                      max-width: 100%;
                    }
                  }
                  .details {
                    position: absolute;
                    left: 0;
                    top: 0;
                    background-color: transparent;
                    width: 100%;
                    height: 100%;
                    align-items: center;
                    text-align: center;
                    .time-status {
                      display: inline-block;
                      position: absolute;
                      bottom: 0;
                      right: 0;
                      color: var(--yt-spec-static-brand-white);
                      background-color: var(--yt-spec-static-overlay-background-heavy);
                      margin-bottom: 10px;
                      span {
                        background-color: rgba(0,0,0, .9);
                        padding: 3px 4px;
                        height: 12px;
                        margin-right: 10px;
                        font-size: 12px;
                        line-height: 12px;
                        color: white;
                        border-radius: 4px;
                        font-weight: 600;
                        font-family: 'Roboto', sans-serif;
                        }
                    }
                  }
                }
                .details {
                  display: flex;
                  a.user-image {
                    display: flex;
                    height: 36px;
                    min-width: 36px;
                    margin-top: 12px;
                    margin-right: 12px;
                    border-radius: 999px;
                    overflow: hidden;
                    image {
                      display: block;
                      margin-left: 12px;
                      margin-right: 12px;
                      max-height: 100%;
                      max-width: 100%;
                    }
                  }
                  .meta {
                    overflow-x: hidden;
                    padding-right: 24px;
                    h3.text-primary {
                      color: $--yt-spec-text-primary;
                      margin: 12px 0 4px 0;
                      a {
                        display: block;
                        font-weight: 600;
                        span {
                          color: $--yt-spec-text-primary;
                          font-family: 'Roboto',sans-serif;
                          font-size: 1rem;
                          line-height: 24px;
                          font-weight: bold;
                          overflow: hidden;
                          display: block;
                          max-height: 4.4rem;
                          -webkit-line-clamp: 2;
                          display: box;
                          display: -webkit-box;
                          -webkit-box-orient: vertical;
                          text-overflow: ellipsis;
                          white-space: normal;
                        }
                      }
                    }
                    .metadata {
                      &-line {
                        span {
                          margin-right: 4px;
                        }
                      }
                    }
                  }
                }
              }
            }
          }
}
</style>

<script lang="ts">
</script>