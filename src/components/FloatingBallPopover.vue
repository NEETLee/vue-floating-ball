<script>
import Emitter from '@/libs/emitter'

export default {
  name: 'FloatingBallPopover',
  mixins: [ Emitter ],
  render: function (createElement) {
    let self = this
    return createElement(
      'div',
      {
        class: {
          'floating-ball-popover': true
        }
      },
      self.popoverEvents.map((eventItem) => {
        return createElement(
          'div',
          {
            class: {
              'floating-ball-popover-item': true
            },
            on: {
              click: () => {
                self.closePopover()
                self.dispatch(eventItem.parentName, eventItem.eventName, self)
              }
            }
          },
          [
            createElement(
              'div',
              {
                class: {
                  'floating-ball-popover-item-icon': true
                }
              },
              [
                createElement(
                  'i',
                  {
                    class: [eventItem.iconName]
                  }
                )
              ]
            ),
            createElement(
              'div',
              {
                class: {
                  'floating-ball-popover-item-word': true
                }
              },
              eventItem.showName
            )
          ])
      })
    )
  },
  props: {
    popoverEvents: {
      type: Array,
      validator (val) {
        return val instanceof Array
      }
    }
  },
  methods: {
    closePopover () {
      this.$parent.isShow = false
      this.$el.style.transform = 'scale(0, 0)'
    }
  }
}
</script>

<style lang="scss" scoped>
.floating-ball-popover {
  position: absolute;
  padding: 0.2rem;
  border-radius: 2rem;
  display: flex;
  flex-flow: row wrap;
  align-content: flex-start;
  transition: transform .2s;
  &-item {
    color: #fff;
    cursor: pointer;
    box-sizing: border-box;
    margin: 0.2rem 0.6rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 3.8rem;
    @mixin popover-common {
      box-sizing: border-box;
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    &-icon {
      @include popover-common;
      height: 3.4rem;
      font-size: 2.3rem;
      // border: 1px #fff solid;
      border-radius: 50%;
    }
    &-word {
      @include popover-common;
      height: 1rem;
      font-size: 1rem;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
  }
}
</style>
