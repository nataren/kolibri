<template>

  <div ref="container" class="container" allowfullscreen>
    <icon-button
      class="btn"
      v-if="fullscreenAllowed"
      :text="isFullScreen ? $tr('exitFullscreen') : $tr('enterFullscreen')"
      @click="toggleFullScreen"
      :primary="true">
      <mat-svg v-if="isFullScreen" class="icon" category="navigation" name="fullscreen_exit"/>
      <mat-svg v-else class="icon" category="navigation" name="fullscreen"/>
    </icon-button>
    <iframe ref="sandbox" class="sandbox" :src="rooturl" sandbox="allow-scripts"></iframe>
  </div>

</template>


<script>

  import ScreenFull from 'screenfull';
  import iconButton from 'kolibri.coreVue.components.iconButton';
  export default {
    components: { iconButton },
    props: {
      defaultFile: {
        type: Object,
        required: true,
      },
    },
    data: () => ({ isFullScreen: false }),
    computed: {
      rooturl() {
        return this.defaultFile.storage_url;
      },
      fullscreenAllowed() {
        return ScreenFull.enabled;
      },
    },
    methods: {
      toggleFullScreen() {
        ScreenFull.toggle(this.$refs.container);
        this.isFullScreen = ScreenFull.isFullscreen;
      },
    },
    mounted() {
      this.$emit('startTracking');
      const self = this;
      this.timeout = setTimeout(() => {
        self.$emit('updateProgress', 1);
      }, 15000);
    },
    beforeDestroy() {
      if (this.timeout) {
        clearTimeout(this.timeout);
      }
      this.$emit('stopTracking');
    },
    $trNameSpace: 'html5Renderer',
    $trs: {
      exitFullscreen: 'Exit fullscreen',
      enterFullscreen: 'Enter fullscreen',
    },
  };

</script>


<style lang="stylus" scoped>

  .btn
    position: absolute
    left: 50%
    transform: translateX(-50%)

  .container
    position: relative
    text-align: center
    height: 100vh
    max-height: calc(100vh - 24em)
    min-height: 400px
    overflow-x: auto
    &:fullscreen
      width: 100%
      height: 100%
      min-height: inherit
      max-height: inherit

  .sandbox
    height: 100%
    width: 100%

</style>
