<template>
  <div class="floating-ball-contain" :ref="id">
    <floating-ball ref="ball"></floating-ball>
    <floating-ball-popover :popover-events="popoverEvents"></floating-ball-popover>
  </div>
</template>

<script>
import FloatBallEvent from '@/libs/floating-ball-event'
import FloatingBall from './FloatingBall'
import FloatingBallPopover from './FloatingBallPopover'

export default {
  name: 'FloatingBallContain',
  components: {
    'floating-ball-popover': FloatingBallPopover,
    'floating-ball': FloatingBall
  },
  props: {
    themeColor: {
      type: String,
      default: '#595857'
    },
    initPosition: {
      type: String,
      default: 'bottom right'
    },
    roll: {
      type: Boolean,
      default: false
    },
    popoverEvents: {
      type: Array,
      validator (val) {
        return val instanceof Array
      },
      default: function () {
        return []
      }
    }
  },
  data () {
    return {
      id: null,
      isShow: false,
      popoverEventsNum: null
    }
  },
  watch: {
    themeColor: function () {
      this.$nextTick(() => {
        FloatBallEvent.init(this, this.themeColor, this.initPosition, 'watch')
      })
    },
    initPosition: function () {
      this.$nextTick(() => {
        FloatBallEvent.init(this, this.themeColor, this.initPosition, 'watch')
      })
    },
    popoverEvents: function () {
      this.popoverEventsNum = this.popoverEvents.length > 4 ? this.popoverEvents.length : 4
      this.$nextTick(() => {
        FloatBallEvent.init(this, this.themeColor, this.initPosition, 'watch')
      })
    },
    roll: function () {
      if (this.roll) {
        this.$refs.ball.$el.style.transform = 'rotate(10000turn)'
        this.$refs.ball.$el.style.transition = '60000s'
      } else {
        this.$refs.ball.$el.style.transform = ''
        this.$refs.ball.$el.style.transition = ''
      }
    }
  },
  created () {
    this.generateID()
    this.popoverEventsNum = this.popoverEvents.length > 4 ? this.popoverEvents.length : 4
  },
  mounted () {
    this.$nextTick(() => {
      FloatBallEvent.init(this, this.themeColor, this.initPosition, 'init')
    })
  },
  methods: {
    generateID () {
      let $chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678'
      let maxPos = $chars.length
      let str = ''
      for (let i = 0; i < 10; i++) {
        str += $chars.charAt(Math.floor(Math.random() * maxPos))
      }
      this.id = str
    }
  }
}
</script>

<style lang="scss" scoped>
.floating-ball-contain {
  position: fixed;
  width: 3.5rem;
  height: 3.5rem;
}
</style>
