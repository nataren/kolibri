<template>

  <div class="wrapper">
    <div class="visuallyhidden" id="progress-bar-label">{{ $tr('label') }}</div>
    <div class="progress-bar-wrapper"
         role="progressbar"
         aria-labelledby="progress-bar-label"
         :aria-valuenow="percent"
         aria-valuemin="0"
         aria-valuemax="100">
      <div class="progress-bar-complete" :style="{ width: percent + '%',  backgroundColor: color}"></div>
    </div>
    <div class="progress-bar-text" v-if="showPercentage">{{ $tr('pct', [progress]) }}</div>
  </div>

</template>


<script>

  export default {
    $trNameSpace: 'progressBar',
    $trs: {
      label: 'Progress:',
      pct: '{0, number, percent}',
    },
    props: {
      progress: {
        type: Number,
        required: true,
      },
      color: {
        type: String,
        required: false,
      },
      showPercentage: {
        type: Boolean,
        required: false,
        default: true,
      },
    },
    computed: {
      percent() {
        return Math.max(Math.min(this.progress * 100, 100), 0);
      },
    },
  };

</script>


<style lang="stylus" scoped>

  @require '~kolibri.styles.definitions'

  .wrapper
    position: relative
    white-space: nowrap
    padding-right: 40px

  .progress-bar-wrapper
    display: inline-block
    position: relative
    width: 100%
    max-width: 125px
    height: 1.2em
    background-color: $core-grey
    border-radius: 15px
    float: left
    margin-right: 5px
    overflow: hidden

  .progress-bar-complete
    height: 100%
    width: 0
    background-color: $core-action-normal
    transition: width, $core-time, ease

  .progress-bar-text
    display: inline-block
    position: relative
    right: 0
    width: 30px
    text-align: left

</style>
