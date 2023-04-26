<script lang="ts">
import { mapGetters } from 'vuex'
import VideoPlayer from '../components/VideoPlayer.vue'

export default {
  computed: {
    ...mapGetters({
      vid: 'v',
      channel: 'channel'
    })
  },
  components: {
    VideoPlayer,

  },
  methods: {
  }
}
</script>

<template>
  <div class="watch">
    <div class="video">
      <VideoPlayer ref="player" />
    </div>
  </div>
</template>

<style lang="scss">
.watch {
  width: 100%;
  height: 100%;

  .video {
    width: 100vw;
    display: flex;
    justify-content: center;
    position: relative;
  }
}
</style>
