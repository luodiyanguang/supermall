<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        scroll: null
      }
    },
    mounted: function () {
      this.scroll = new BScroll(this.$refs.wrapper, {
        click: true,
        tap: true,
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad
      })

      //监听滚动的位置
      if (this.probeType === 2 || this.probeType === 3) {
        this.scroll.on('scroll', position => {
          this.$emit('scroll', position)
        })
      }

      //监听滚动到底部事件
      if (this.pullUpLoad) {
        this.scroll.on("pullingUp", () => {
          // console.log('上拉加载更多');
          this.$emit('pullingUp')
        })
      }
    },
    methods: {
      scrollTo(x , y , time = 200) {
        this.scroll.scrollTo(x , y , time)
      },
      finishPullUp() {
        this.scroll && this.scroll.finishPullUp()
      },
      refresh() {
        // console.log('----')
        this.scroll && this.scroll.refresh && this.scroll.refresh()
      },
      getScrollY() {
        return this.scroll ? this.scroll.y : 0
      }
    }
  }
</script>

<style scoped>

</style>