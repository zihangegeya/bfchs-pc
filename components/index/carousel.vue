<template>
  <el-carousel :interval="4000"  height="35rem">
    <el-carousel-item v-for="(item,index) in bannerList" :key="index">
      <img :src="item.bannerUrl"  style="width: 100%;height: 100%;display: inline-block;"/>
    </el-carousel-item>
  </el-carousel>
</template>

<script>

  if (process.browser) { // 在这里根据环境引入wow.js
    var {WOW} = require('wowjs')
  }

  export default {
    props: {
      size: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
        slide: 0,
        sliding: null,
        bannerList: [],
        swiperOption: {
          //显示分页
          pagination: {
            el: '.swiper-pagination'
          },
          //设置点击箭头
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          },
          //自动轮播
          autoplay: {
            delay: 2000,
            //当用户滑动图片后继续自动轮播
            disableOnInteraction: false,
          },
          //开启循环模式
          loop: true
        }
      }
    },
    mounted() {
      if (process.browser) {  // 在页面mounted生命周期里面 根据环境实例化WOW
        new WOW({}).init()
      }
      ;
      this.getActiveAd();


    },
    methods: {
      onSlideStart(slide) {
        this.sliding = true
      },
      onSlideEnd(slide) {
        this.slide = slide
        this.sliding = false
      },
      getActiveAd() {
        this.$axios.post("/banner/getActiveAd", {type: '1'}).then(res => {
          if (res.data.code == 200) {
            this.bannerList = res.data.data;
          }
          // console.log(res);
          // debugger;
        }).catch(err => {

        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '@/assets/styles/variables.scss';

  .carousel .mask {
    position: absolute;
    top: 0;
    right: 15%;
    bottom: 0;
    left: 15%;
    color: #fff;
    font-size: 20px;
    z-index: 9;

    .content {
      position: absolute;
      right: 8%;
      top: 50%;
      width: 50%;
      transform: translateY(-50%);
      text-align: right;

      .title {
        font-size: 45px;
        color: #13d0b0;
        letter-spacing: 8px;
        font-family: "Segoe UI", "Lucida Grande", Helvetica, Arial, "Microsoft YaHei", FreeSans, Arimo, "Droid Sans", "wenquanyi micro hei", "Hiragino Sans GB", "Hiragino Sans GB W3", Roboto, Arial, sans-serif;
      }

      .desc {
        font-size: 24px;
        font-weight: lighter;
        letter-spacing: 3px;
      }

      .line {
        // display: inline-block;
        // width: 100px;
        // height: 2px;
        // margin-top: 20px;
        font-size: 18px;
        font-weight: lighter;
        opacity: 0.9;
        // background: #fff;
      }
    }

    .content2 {
      left: 8%;
      text-align: left;
    }
  }

  @media screen and (max-width: 1400px) {
    .carousel .mask {
      .content {
        .title {
          font-size: 32px;
        }

        .desc {
          font-size: 18px;
        }

        .line {
          font-size: 14px;
        }
      }
    }
  }

  @media screen and (max-width: 1200px) {
    .carousel .mask {
      .content {
        .title {
          font-size: 34px;
        }

        .desc {
          font-size: 16px;
        }

        .line {
          font-size: 14px;
        }
      }
    }
  }

  @media screen and (max-width: 992px) {
    .carousel .mask {
      .content {
        .title {
          font-size: 26px;
        }

        .desc {
          font-size: 14px;
        }

        .line {
          font-size: 12px;
        }
      }
    }
  }

  @media screen and (max-width: 768px) {
    .mask2 {
      left: 0 !important;
    }
    .carousel .mask {
      right: 0;

      .content {
        top: 55%;
        width: 70%;

        .title {
          font-size: 26px;
          opacity: 0.8;
          letter-spacing: 2px;
        }

        .desc {
          font-size: 14px;
          letter-spacing: 1px;
        }

        .line {
          // width: 50px;
          // height: 1px;
          margin-top: 0;
        }
      }

      // .content2 {
      //   top: 30%;
      // }
    }
  }
</style>
