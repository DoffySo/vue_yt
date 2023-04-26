<script setup lang="ts">
import {ref} from 'vue'

const myObject = ref([
  {
    title: "Nigga",
    thumbnailUrl: "https://i.ytimg.com/vi/RS1_ZmS7SV4/hq720.jpg?sqp=-oaymwEcCNAFEJQDSFXyq4qpAw4IARUAAIhCGAFwAcABBg==&rs=AOn4CLBaTf87z_w5npbwyX3uNqxc3yxfkA",
    avatarUrl: "https://yt3.ggpht.com/hqcIJtUa4Q0VHv0_F_6iaCCl_dapROhVKn_ixvxs4At9VJg7Ijw9GKcA53ywWOt_sh5FK7Ur=s68-c-k-c0x00ffffff-no-rj",
    views: "1488",
    timeago: "1488 hours ago",
    time: "14:88",
    author: "Hitler",
    linkToVid: "djgTOPEG_536q"
  }
])
</script>

<script lang="ts">
import { defineComponent } from 'vue';

import Video from '../components/Video.vue'

export default defineComponent({
  name: "contents",
  components: {
    Video,
  }
})

</script>
<template>
  <div class="contents">
    <div class="videos">
        <div class="contents-row">
          <Video v-for="item in myObject" :link-to-vid=item.linkToVid :title=item.title :author=item.author :thumbnail-url=item.thumbnailUrl :avatar-url=item.avatarUrl :time=item.time :timeago=item.timeago :views=item.views />
        </div>
    </div>
  </div>
</template>


<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat&family=Roboto&display=swap');

$--ytd-rich-grid-items-per-row: 3;
$--ytd-rich-grid-item-max-width: 360px;
$--ytd-rich-grid-item-margin: 16px;
$--yt-button-compact-background-color: 0;
$--yt-button-compact-text-color: 0;
$--yt-spec-text-primary: #f1f1f1;

  .contents {
    width: 100%;
    padding-top: 24px;
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;

    .videos {
      width: 100%;
      display: flex;
      justify-content: center;
      .contents-row {
        width: 100%;
        max-width: calc($--ytd-rich-grid-items-per-row * ($--ytd-rich-grid-item-max-width + $--ytd-rich-grid-item-margin));
        margin: 0 16px;
        display: flex;
        justify-content: start;
        flex-wrap: wrap;

        .video {
          width: calc(100%/$--ytd-rich-grid-items-per-row - $--ytd-rich-grid-item-margin - 0.01px)
        }
      }
    }
  }
</style>
