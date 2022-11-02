
<template>
  <div id="app">
    <iframe
      :src="urlData"
      :width="width"
      :height="height"
      allowfullscreen
      frameborder="0"
      allow="autoplay"
    ></iframe>
  </div>
</template>

<script>
// Imports
import { ConfigLoader } from '@igappstore/core'

export default {
  name: 'App',
  data() {
    return {
      config: window.MicroAppConfiguration,
      tempconfig: window.tempconfig,
      urlData: '',
      height: '',
      width: '',
      videoId: '',
    }
  },
  watch: {
    config(newVal, oldVal) {
      this.tempconfig = ''
      this.tempconfig = this.config
      this.videoId = ''
      this.videoId = this.tempconfig.youtubeurl.split('v=')[1].substring(0, 11)
      this.tempconfig.youtubeurl = this.tempconfig.youtubeurl.replace(
        'watch?v=',
        'embed/'
      )
    },
  },
  methods: {
    async checkUrl() {
      const config = await ConfigLoader.loadConfig()
      this.config = config
    },
    async getConfig() {
      let params = this.tempconfig
      this.width = window.innerWidth
      this.height = window.innerHeight
      this.myEventHandler()
      if (this.tempconfig.loop == 1) {
        if (this.tempconfig.subtitles == 1) {
          this.urlData =
            this.tempconfig.youtubeurl +
            `?autoplay=1` +
            `&end=${this.tempconfig.maxduration}&mute=${this.tempconfig.mute}&cc_load_policy=${this.tempconfig.subtitles}&iv_load_policy=${this.tempconfig.subtitles}&cc_lang_pref=en&hl=en&persist_hl=1&loop=${this.tempconfig.loop}&playlist=${this.videoId}`
        } else {
          this.urlData =
            this.tempconfig.youtubeurl +
            `?autoplay=1` +
            `&end=${this.tempconfig.maxduration}&mute=${this.tempconfig.mute}&cc_load_policy=3&iv_load_policy=3&cc_lang_pref=en&hl=en&persist_hl=1&loop=${this.tempconfig.loop}&playlist=${this.videoId}`
        }
      } else {
        if (this.tempconfig.subtitles == 1) {
          this.urlData =
            this.tempconfig.youtubeurl +
            `?autoplay=1` +
            `&end=${this.tempconfig.maxduration}&mute=${this.tempconfig.mute}&cc_load_policy=${this.tempconfig.subtitles}&iv_load_policy=${this.tempconfig.subtitles}&cc_lang_pref=en&hl=en&persist_hl=1&loop=${this.tempconfig.loop}`
        } else {
          this.urlData =
            this.tempconfig.youtubeurl +
            `?autoplay=1` +
            `&end=${this.tempconfig.maxduration}&mute=${this.tempconfig.mute}&cc_load_policy=3&iv_load_policy=3&cc_lang_pref=en&hl=en&persist_hl=1&loop=${this.tempconfig.loop}`
        }
      }
      // console.log(this.urlData)
      // this.urlData =
      //   this.tempconfig.youtubeurl +
      //   `?autoplay=1` +
      //   `&end=${this.tempconfig.maxduration}&mute=${this.tempconfig.mute}&cc_load_policy=${this.tempconfig.subtitles}&cc_lang_pref=en&loop=${this.tempconfig.loop}&rel=0&playlist=${this.videoId}`
    },
    myEventHandler() {
      this.width = window.innerWidth
      this.height = window.innerHeight
    },
  },
  components: {},
  async created() {
    await this.checkUrl()
    await this.getConfig()
    window.addEventListener('resize', this.myEventHandler)
  },
  destroyed() {
    window.removeEventListener('resize', this.myEventHandler)
  },
}
</script>

<style>
html {
  overflow: hidden !important;
  height: 100%;
  width: 100%;
}
body {
  margin: 0;
  height: 100%;
  width: 100%;
}
</style>