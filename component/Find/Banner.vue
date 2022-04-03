<template>
<!--轮播图组件-->
  <div class="banner_wrapper">
    <ul class="banner_inner_wrapper"
        :style="{left: `${bannerData.innerWrapperLeftValue}vw`, transition: bannerData.innerTransition}"
        @touchstart="handleBannerTouchStart"
        @touchmove="handleBannerTouchMove"
        @touchend="handleBannerTouchEnd">
      <li class="banner_item">
        <a href="#" class="banner_link">
          <img v-bind:src="bannerList[0]? bannerList[bannerList.length - 1].pic : ''" alt="">
          <div class="type_text" :style="{backgroundColor: bannerList[0]? bannerList[bannerList.length - 1].titleColor : ''}">
            {{bannerList[0]? bannerList[bannerList.length - 1].typeTitle : ''}}
          </div>
        </a>
      </li>
      <li class="banner_item" v-for="(item, index) in bannerList">
        <a href="#" class="banner_link">
          <img v-bind:src="item.pic" alt="">
          <div class="type_text" :style="{backgroundColor: item.titleColor}">
            {{item.typeTitle}}
          </div>
        </a>
      </li>
      <li class="banner_item">
        <a href="#" class="banner_link">
          <img v-bind:src="bannerList[0]? bannerList[0].pic : ''" alt="">
          <div class="type_text" :style="{backgroundColor: bannerList[0]? bannerList[0].titleColor : ''}">
            {{bannerList[0]? bannerList[0].typeTitle : ''}}
          </div>
        </a>
      </li>
    </ul>
    <div class="banner_dop_wrapper">
      <div class="banner_dop" v-for="(item, index) in bannerList"
           :class="{banner_dop_active: index === bannerData.currentIndex - 1 }"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Banner",
  data() {
    return {
      //存储轮播图的数据
      bannerList: [],
      //轮播图播放的数据
      bannerData: {
        // 当前显示的轮播图的索引
        currentIndex: 1,
        // 轮播图定时器
        bannerTimer: null,
        // ul 标签的 left 值(由于不能操作dom，只能写在这里)
        innerWrapperLeftValue: -95,
        // touchStart 时候 的 innerWrapperLeftValue 值
        touchedInnerWrapperLeftValue: 0,
        //内部ul容器的transition值
        innerTransition : `.5s`,
        //手指是否处于触摸中
        isTouching: false,
        //视口宽度
        screenWidth: 0,
        //轮博图触摸开始时的位置X
        touchStartPositionX: 0,
        // 用于轮播图手指松开时，滑到下一页还是上一页
        slideNextOrLastPage: 0
      }
    }
  },
  methods: {
    /**
     * 获取视口的宽度
     * */
    async getViewportWidth() {
      await uni.getSystemInfo({
        success: (res) => {
          this.bannerData.screenWidth = res.screenWidth;
          //输出获取到的视口宽度
          // console.log(res.screenWidth);
        }
      });
    },

    /**
     * 获取轮播图数据
     * */
    async getBannerList() {
      console.log(this.$store.state.localhost);
      let res = await uni.request({
        url: `${this.$store.state.localhost}/banner?type=1`,
        method: 'get',
        params: {
          type: `1`, //1 表示安卓设备
        },
        success: (res) => {
          this.bannerList = res.data.banners
          //输出获取到的轮播图数据
          // console.log(this.bannerList)
        }
      })
    },

    /**
     * 轮播图自动播放
     * */
    bannerPlay() {
      this.startBannerTimer()
    },

    /**
     * 开启轮播图自动播放定时器的函数
     * */
    startBannerTimer() {
      this.bannerData.bannerTimer = setInterval(async () => {
        this.bannerData.currentIndex += 1;
        // console.log(this.bannerData.currentIndex);
        this.bannerData.innerWrapperLeftValue = -95*this.bannerData.currentIndex
        if(this.bannerData.currentIndex === this.bannerList.length + 1) {
          //延时过动画跳过的时间
          setTimeout( () => {
            this.bannerData.innerTransition = `none`;
            this.bannerData.currentIndex = 1;
            this.bannerData.innerWrapperLeftValue = -95*this.bannerData.currentIndex
            //TODO 不知道这里为什么加上setTimeout bug就解决了
            setTimeout(() => { this.bannerData.innerTransition = `.5s`}, 100)
          }, 500)
        }
      }, 3000)
    },

    /**
     * 处理手指开始触摸事件
     * */
    handleBannerTouchStart(event) {
      // console.log(`touch start`);
      // console.log(event.touches[0].pageX);
      this.bannerData.touchStartPositionX = event.touches[0].pageX
      // 触摸开始时，清除自动播放定时器
      clearInterval(this.bannerData.bannerTimer)
      this.bannerData.touchedInnerWrapperLeftValue = this.bannerData.innerWrapperLeftValue
      // 关闭动画，防止出现拖不动的情况
      this.bannerData.innerTransition = `none`
    },

    /**
     * 处理轮博图touchmove事件
     * */
    handleBannerTouchMove(event) {
      // console.log(`touch move`);
      // console.log(event);
      // this.innerWrapperLeftValue
      //改变的像素值
      let changedValuePx = event.touches[0].pageX - this.bannerData.touchStartPositionX
      // 将改变的值换算为vw
      let changedValueVw = changedValuePx / this.bannerData.screenWidth * 100
      // 用于松开手指时，判断是上一页还是下一页
      if(changedValuePx > 0) this.bannerData.slideNextOrLastPage = 0
      else this.bannerData.slideNextOrLastPage = 1
      this.bannerData.innerWrapperLeftValue =  this.bannerData.touchedInnerWrapperLeftValue + changedValueVw
    },

    /**
     * 处理轮播图的touchend事件
     * */
    handleBannerTouchEnd(event) {
      // 手指松开时，进行翻页
      if(this.bannerData.slideNextOrLastPage) this.bannerData.currentIndex += 1
      else this.bannerData.currentIndex -= 1
      this.bannerData.innerTransition = `0.5s`
      this.bannerData.innerWrapperLeftValue = -95*this.bannerData.currentIndex
      //从第一页翻页到最后一页
      if(this.bannerData.currentIndex === 0) {
        //等待翻页动画播完
        setTimeout(() => {
          this.bannerData.innerTransition = `none`
          this.bannerData.currentIndex = this.bannerList.length
          this.bannerData.innerWrapperLeftValue = -95*this.bannerData.currentIndex
          setTimeout(() => { this.bannerData.innerTransition = `.5s`}, 100)
        }, 100)
        this.startBannerTimer()
      }else if(this.bannerData.currentIndex === this.bannerList.length + 1) {
        //等待翻页动画播完
        setTimeout(() => {
          this.bannerData.innerTransition = `none`
          this.bannerData.currentIndex = 1
          this.bannerData.innerWrapperLeftValue = -95*this.bannerData.currentIndex
          setTimeout(() => { this.bannerData.innerTransition = `.5s`}, 100)
        }, 100)
        this.startBannerTimer()
      } else {
        this.startBannerTimer();
      }
    },
  },

  async beforeMount() {
    await this.getBannerList()
    this.bannerPlay()
  },

  async mounted() {
    await this.getViewportWidth()
  },
}
</script>

<style scoped lang="less">
  .banner_wrapper {
    width: 100vw;
    height: 35vw;
    overflow: hidden;
    margin-top: 3vw;
    position: relative;

    .banner_inner_wrapper {
      list-style: none;
      display: flex;
      flex-wrap: nowrap;
      flex-direction: row;
      padding: 0 2.5vw;
      position: relative;
      //left: -90vw;
      //transition: .5s;

      .banner_item {
        width: 90vw;
        margin-left:2.5vw;
        margin-right:2.5vw;
        position: relative;

        a {
          display: block;
        }

        img {
          width: 90vw;
          height: 35vw;
          border-radius: 3vw;
        }

        .type_text {
          position: absolute;
          right:0;
          bottom: 0;;
          text-decoration: none;
          width: 14vw;
          height: 5vw;
          line-height: 5vw;
          font-size: 3vw;
          text-align: center;
          // border: 2px solid #000;
          border-radius: 3vw 0 3vw 0;
          color: #fff;
        }
      }
    }

    .banner_dop_wrapper {
      width: 100vw;
      height:3vw;
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      position: absolute;
      left: 0;
      bottom: 2vw;

      .banner_dop {
        width: 3vw;
        height: 1vw;
        background: rgba(255,255,255,.5);
        margin-left: 1vw;
        margin-right: 0.1vw;
        border-radius: 1vw
      }

      .banner_dop_active {
        background: rgba(255,255,255,1);
      }
    }
  }
</style>
