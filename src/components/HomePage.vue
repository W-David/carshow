<template>
  <div class="container">
    <div class="rc-header">
      <div class="rc-header-left">
        <img class="rc-logo" :src="media.LOGO_PATH">
        <img class="rc-slogan" :src="media.SLOGAN_PATH">
      </div>
      <div class="rc-header-right">
        <a href="#"
           :class="{'active': lang === 'chinese'}"
           @click="toggleLang('chinese')">
          中文
        </a>
        &nbsp;/
        <a href="#"
           :class="{'active': lang === 'english'}"
           @click="toggleLang('english')"> 
          English
        </a>
      </div>
    </div>
    <div class="swiper-container rc-body">
      <swiper ref="pageSwiper" :options="pageSwiperOptions">
        <swiper-slide>
          <div class="rc-page rc-page01">
            <div class="rc-content-wrapper">
              <div class="rc-bg rc-layer-bg"
                   ref="bgGalaxy"
                   :style="{backgroundImage: 'url(' + common.GALAXY_LAYER_BG + ')',
                            top: layerEdge,
                            right: layerEdge,
                            bottom: layerEdge,
                            left: layerEdge}">
              </div>
              <div class="rc-bg rc-layer-top"
                   ref="topGalaxy"
                   :style="{backgroundImage: 'url(' + common.GALAXY_LAYER_TOP + ')'}">
              </div>
              <div class="rc-text-wrapper"
                   :style="{visibility: pageIndex === 0 ? 'visible' : 'hidden'}">
                <h2 ref="pageOneSlogan">{{ langs.PAGE_ONE_SLOGAN }}</h2>
                <scroll-number :size="isMobile ? 18 : 24" 
                               ref="refOfScrollNumber">
                  <template v-slot:prefix>
                    <span style="color: #c3c3c3;">
                      {{ langs.PAGE_ONE_SUP_SLOGAN }}
                    </span>
                  </template>
                  <template v-slot:suffix>
                    <span style="color: #c3c3c3;">
                      {{ langs.PAGE_ONE_SUB_SLOGAN }}
                    </span>
                  </template>
                </scroll-number>
                <button class="btn btn-try"
                        ref="btnTry"
                        :style="{visibility: pageIndex === 0 ? 'visible' : 'hidden'}">
                  {{ langs.TRY_LABEL }}
                </button>
              </div>
            </div>
            <img :src="common.GALAXY_REAL_TOP" v-show="false">
            <img :src="common.GALAXY_REAL_BG" v-show="false">
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="rc-page rc-page02">
            <div class="rc-content-wrapper">
              <div class="rc-text-wrapper"
                   :style="{visibility: pageIndex === 1 ? 'visible' : 'hidden'}"
                   ref="pageTwoSlogan">
                <h2>{{ langs.PAGE_TWO_SLOGAN }}</h2>
              </div>
              <div class="rc-step-wrapper">
                <div class="rc-step-stage">
                  <div class="swiper-container rc-step-container">
                    <swiper ref="stepSwiper" :options="stepSwiperOptions">
                      <swiper-slide v-for="(step, index) in langs.STEPS_PROFILE"
                                    :key="index">
                        <div class="rc-step-content">
                          <h2 class="rc-step-title">{{ langs.STEP_LABEL }}{{ index + 1 }}</h2>
                          <p class="rc-step-profile">{{ step }}</p>
                          <button class="btn"
                                  :ref="`btnRefOf${index}`"
                                  v-show="index === 0 || index === 1">{{ index === 0 ? langs.DOWNLOAD_LABEL : langs.AUTH_LABEL }}
                          </button>
                        </div>
                      </swiper-slide>
                      <div class="swiper-button-next" slot="button-next" v-if="!isMobile"></div>
                      <div class="swiper-button-prev" slot="button-prev" v-if="!isMobile"></div>
                    </swiper>
                  </div>
                </div>
                <div class="rc-step-scene" v-if="!isMobile">
                  <div :style="{backgroundImage: 'url(' + media.STEPS_ACTOR_PATH[stepIndex] + ')'}"
                       class="rc-bg rc-step-actor">
                  </div>
                </div>
              </div>
            </div>
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="rc-page rc-page03">
            <div class="rc-content-wrapper">
              <div class="rc-text-wrapper" 
                   :style="{visibility: pageIndex === 2 ? 'visible' : 'hidden'}"
                   ref="pageThreeSlogan">
                <h2>{{ langs.PAGE_THREE_SLOGAN }}</h2>
              </div>
              <div class="rc-cars-gallery">
                <div v-for="(car, index) in langs.CARS_PROFILE"
                     :key="index"
                     class="rc-car-card"
                     :title="langs.CARS_TOOLTIP">
                  <div class="rc-car-media">
                    <div :style="{backgroundImage: 'url(' + common.CARS_PATH[index] + ')'}"
                         class="rc-bg rc-bg-scale rc-car-snapshot">
                    </div>
                    <div class="rc-car-profile" @mouseenter="hoverProfile">
                      <h4>{{ car.NAME }}</h4>
                      <div>{{ langs.OWNER_LABEL }}:&nbsp;{{ car.OWNER }}</div>
                      <div>{{ langs.DATE_LABEL }}:&nbsp;{{ car.DATE }}</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="rc-page rc-page04">
            <div class="rc-content-wrapper">
              <div class="rc-text-wrapper"
                   :style="{visibility: pageIndex === 3 ? 'visible' : 'hidden'}"
                   ref="pageFourSlogan">
                <h2>{{ langs.PAGE_FOUR_SLOGAN }}</h2>
              </div>
              <div class="rc-bg rc-drive-stage">
                <div :style="{backgroundImage: 'url(' + common.DRIVE_STAGE_ACTOR + ')'}"
                     class="rc-bg rc-actor-car"
                     ref="driveStageActor">
                </div>
              </div>
            </div>
          </div>
        </swiper-slide>
        <div class="swiper-pagination" slot="pagination" v-show="!isMobile"></div>
      </swiper>
    </div>
  </div>
</template>

<script>
  import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'
  import 'swiper/css/swiper.css'
  import 'animate.css/animate.min.css'
  import config from '../config'
  import ScrollNumber from '../widgets/ScrollNumber'

  export default {
    name: 'HomePage',
    components: {
      Swiper,
      SwiperSlide,
      ScrollNumber
    },
    directives: {
      swiper: directive
    },
    data () {
      return {
        lang: 'chinese',
        // bug layerEdge 在beforeCreate中创建，data在created之后生成，导致layerEdge始终为0
        pageIndex: 0,
        stepIndex: 0,
        pageSwiperOptions: {
          speed: 800,
          direction: 'vertical',
          mousewheel: true,
          pagination: {
            el: '.swiper-pagination',
            clickable: true
          },
          on: {
            slideChangeTransitionEnd: this.onSlideChangeTransitionEnd           
          }
        },
        stepSwiperOptions: {
          speed: 600,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          },
          on: {
            slideChangeTransitionEnd: this.onInsideChangeTransitionEnd
          }
        }
      }
    },
    beforeCreate () {
      let w = document.body.clientWidth
      let h = document.body.clientHeight
      this.layerEdge = `calc(50% - ${Math.ceil(Math.sqrt(w * w + h * h) / 2)}px)`
    },
    mounted () {
      this.$nextTick(function () {
        this.initPage()
      })
    },
    methods: {
      hoverProfile (event) {
        this.bindAnimation(event.target, 'heartBeat')
      },
      onSlideChangeTransitionEnd () {
        let pageSwiper = this.pageSwiper
        this.pageIndex = pageSwiper.activeIndex
        this.animatePage()
      },
      onInsideChangeTransitionEnd () {
        let stepSwiper = this.stepSwiper
        if(stepSwiper.activeIndex === 0 || stepSwiper.activeIndex === 1) {
          this.bindAnimation(this.$refs[`btnRefOf${stepSwiper.activeIndex}`][0], 'tada')
        }
        this.stepIndex = stepSwiper.activeIndex 
      },
      toggleLang (lang) {
        this.lang = lang
        this.animatePage()
      },
      addClass (el, cls) {
        let elClassArr = el.className.split(' ')
        let classArr = cls.split(' ')
        classArr.forEach(item => {
          if(elClassArr.indexOf(item) === -1) {
            elClassArr.push(String(item))
          }
        })
        el.className = elClassArr.join(' ')
        return el
      },
      removeClass (el, cls) {
        let elClassArr = el.className.split(' ')
        let classArr = cls.split(' ')
        classArr.forEach(item => {
          let index = elClassArr.indexOf(item)
          if(index > -1) {
            elClassArr.splice(index, 1)
          }
        })
        el.className = elClassArr.join(' ')
        return el
      },
      bindAnimation (el, x) {
        let _self = this
        let events = [
          'webkitAnimationEnd',
          'mozAnimationEnd',
          'MSAnimationEnd',
          'onanimationend',
          'animationend'
        ]
        _self.addClass(el, 'animate__' + x + ' ' + 'animate__animated')
        events.forEach(event => {
          let func = function () {
            events.forEach(item => {
              el.removeEventListener(item, func)
            })
            _self.removeClass(el, 'animate__' + x + ' ' + 'animate__animated')
          }
          el.addEventListener(event, func)
        })
      },
      initPage () {
        setTimeout(() => {
          this.$refs.topGalaxy.style.backgroundImage = `url(${this.common.GALAXY_REAL_TOP})` 
          this.$refs.bgGalaxy.style.backgroundImage = `url(${this.common.GALAXY_REAL_BG})` 
        }, 1800)
        this.animatePage01()
      },
      animatePage01 () {
        this.bindAnimation(this.$refs.pageOneSlogan, 'bounceInLeft')
        this.bindAnimation(this.$refs.btnTry, 'bounceInRight')
        this.$refs.refOfScrollNumber.$emit('start')
      },
      animatePage02 () {
        this.bindAnimation(this.$refs.pageTwoSlogan, 'fadeInDown')
      },
      animatePage03 () {
        this.bindAnimation(this.$refs.pageThreeSlogan, 'flipInY')
      },
      animatePage04 () {
        this.bindAnimation(this.$refs.pageFourSlogan, 'jackInTheBox')
        setTimeout(() => {
          this.addClass(this.$refs.driveStageActor, 'rc-arrive')
        }, 600)
      },
      animatePage (index = this.pageIndex) {
        switch (index) {
        case 0: 
          this.animatePage01()
          break
        case 1:
          this.animatePage02()
          break
        case 2: 
          this.animatePage03()
          break
        case 3: 
          this.animatePage04()
          break
        default:
          break
        }
      }
    },
    computed: {
      pageSwiper () {
        return this.$refs.pageSwiper.$swiper
      },
      stepSwiper () {
        return this.$refs.stepSwiper.$swiper
      },
      langs () {
        return config.langs[this.lang]
      },
      media () {
        return config.media[this.lang]
      },
      common () {
        return config.common
      },
      isMobile () {
        return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)
      }
    }
  }
</script>

<style lang="less" scoped>
  .container {
    font-family: Roboto, sans-serif;
    height: 100%;
    user-select: none;
    box-sizing: border-box;

    .btn {
      padding: 5px 16px;
      outline: none;
      border: none;
      border-radius: 5px;
      line-height: 20px;

      &:hover {
        background-color: #d5d5d5;
      }
    }


    .rc-header {
      height: 60px;
      line-height: 60px;
      padding: 10px 40px;
      overflow: hidden;

      .rc-header-left {
        float: left;
        height: 100%;
        width: 90%;
        overflow: hidden;

        .rc-logo {
          height: 100%;
        }

        .rc-slogan {
          float: right;
          height: calc(100% - 30px);
          padding: 15px 0;
          // margin-right: 80px;
        }
      }

      .rc-header-right {
        float: right;
        width: 10%;
        height: 100%;
        text-align: right;
        cursor: default;

        a {
          display: inline-block;
          text-decoration: none;
          cursor: pointer;
          color: #888;
          outline: 0;

          &:hover {
            color: #444;
          }

          &.is-active {
            color: #398bfb;
          }
        }
      }
    }

    .rc-body {
      width: 100%;
      height: calc(100% - 80px);
      margin-left: auto;
      margin-right: auto;

      .swiper-container {
        height: 100%;
      }

      .rc-page {
        position: relative;
        height: 100%;
        overflow: hidden;
      }

      .rc-bg {
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center;
        transition: all 320ms linear;

        &.rc-bg-scale:hover {
          transform: scale(1.25);
          filter: blur(1px);
        }
      }

      .rc-page01 {
        .rc-content-wrapper {
          .rc-layer-bg {
            position: absolute;
            animation: rotate 300s infinite;
            -webkit-animation: rotate 300s infinite;
            z-index: 1;
          }

          .rc-layer-top {
            position: absolute;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            z-index: 2;
          }

          .rc-text-wrapper {
            position: absolute;
            text-align: center;
            right: 0;
            left: 0;
            top: 10%;
            z-index: 3;
            padding: 12px 0;
            line-height: 30px;

            h2 {
              margin-bottom: 8px;
              font-size: 26px;
              line-height: 40px;
              color: #e5e5e5;
            }

            .btn-try {
              margin-top: 15px;
              width: 160px;
              background-color: #1ab30b;
              color: #fff;

              &:hover {
                background-color: #00a532;
              }
            }
          }
        }
      }

      .rc-page02 {
        background: rgb(226, 239, 255);
        background: radial-gradient(ellipse at center, rgba(226, 239, 255, 1), 0%, rgba(181, 205, 229, 1) 99%);
        filter: progid:dximagetransform.microsoft.gradient(startColorstr='#e2efff', endColorstr='#b5cde5', GradientType=1); /* IE6-9 fallback on horizontal gradient */
        .rc-content-wrapper {
          height: calc(100% - 60px);
          padding: 30px;

          .rc-text-wrapper {
            line-height: 80px;

            h2 {
              font-size: 32px;
              text-align: center;
              color: #3c3c3c;
            }
          }

          .rc-step-wrapper {
            margin: 15px auto;
            width: 80px;
            min-width: 1000px;
            height: calc(100% - 110px);
            overflow: hidden;

            .rc-step-stage {
              position: relative;
              float: left;
              width: 40%;
              height: 100%;

              .rc-step-container {
                position: absolute;
                width: 100%;
                height: 100%;
                margin: auto;
                top: 0;
                bottom: 0;
                max-height: 400px;

                .rc-step-content {
                  position: absolute;
                  box-sizing: border-box;
                  padding: 15px 70px;
                  height: 40%;
                  min-height: 300px;
                  margin: auto;
                  top: 0;
                  bottom: 0;
                  text-align: center;

                  .rc-step-title {
                    margin: 5px auto;
                    width: 140px;
                    font-size: 20px;
                    line-height: 36px;
                    background-color: #4b8bf5;
                    border-radius: 20px;
                    color: #fff;
                  }

                  .rc-step-profile {
                    margin-top: 30px;
                    font-size: 14px;
                    letter-spacing: 1.5px;
                    line-height: 28px;
                    min-height: 80px;
                  }
                }
              }
            }

            .rc-step-scene {
              position: relative;
              height: 100%;
              overflow: hidden;

              .rc-step-actor {
                position: absolute;
                margin: auto;
                width: calc(100% - 120px);
                height: calc(100% - 60px);
                max-height: 360px;
                // left: 0;
                top: 0;
                bottom: 0;
                right: 0;
                background-size: contain !important;
              }
            }
          }
        }
      }

      .rc-page03 {
        .rc-content-wrapper {
          position: absolute;
          margin: auto;
          top: 0;
          bottom: 0;
          left: 0;
          right: 0;

          .rc-text-wrapper {
            margin: 15px 0;

            h2 {
              text-align: center;
              font-size: 26px;
              line-height: 50px;
              color: #3c3c3c;
            }
          }

          .rc-cars-gallery {
            margin: 10px auto 0;
            padding: 15px;
            width: calc(100% - 80px);
            height: calc(100% - 80px);
            display: flex;
            justify-content: space-around;

            .rc-car-card {
              flex: 1;
              cursor: pointer;
              position: relative;
              width: 100%;
              height: 100%;

              .rc-car-media {
                padding: 5px;

                .rc-car-snapshot {
                  margin: 15px auto;
                  width: 300px;
                  height: 400px;
                  border-radius: 5px;
                }

                .rc-car-profile {
                  text-align: center;
                  padding: 5px;
                  width: 80%;
                  height: 20%;
                  font-size: 0.85em;
                  color: white;
                  line-height: 1.2;
                  position: absolute;
                  top: 30%;
                  right: 10%;
                  z-index: 1;

                  h4 {
                    font-size: 1.15em;
                    font-family: 'Times New Roman', Times, serif;
                  }
                }
              }
            }
          }
        }
      }

      .rc-page04 {
        .rc-content-wrapper {
          .rc-text-wrapper {
            h2 {
              margin-top: 40px;
              text-align: center;
              font-size: 48px;
              line-height: 80px;
              color: #3c3c3c;
            }
          }

          height: calc(100% - 130px);
          padding: 15px 30px;

          .rc-drive-stage {
            position: relative;
            height: 100%;

            .rc-actor-car {
              position: absolute;
              top: 10%;
              right: 0;
              width: 500px;
              height: 225px;
              transform: scale(0.01);
              transition: all 1200ms ease-in;
            }

            .rc-arrive {
              top: calc(50% - 135px);
              right: calc(50% - 250px);
              transform: scale(1.2);
            }
          }
        }
      }
    }
  }

  @media screen and (max-width: 415px) {
    h2 {
      font-size: 22px;
      color: #4285f4;
    }

    .rc-header {
      height: 29px;
      line-height: 30px;
    }

    .rc-body {
      height: calc(100% - 50px);


      .rc-page01 {
        .rc-content-wrapper {
          height: 220px;

          .rc-text-wrapper {
            top: calc(50% - 10px);
          }
        }
      }

      .rc-page02 {
        .rc-content-wrapper {
          top: 80px;
          padding: 30px 0;

          .rc-step-wrapper {
            min-width: 100%;

            .rc-step-stage {
              width: 100%;

              .rc-step-content {
                height: 100%;
              }
            }
          }
        }
      }

      .rc-page03 {
        .rc-content-wrapper {
          .rc-text-wrapper {
            margin: 0;
          }

          .rc-cars-gallery {
            width: 100%;
            height: calc(100% - 50px);

            .rc-car-card {
              width: 100%;
              height: 100px;
              overflow: hidden;
              border-radius: 0;

              .rc-car-media {
                height: 100%;
                width: 50%;
                float: left;
                border-radius: 0;
                padding: 0;

                .rc-car-profile {
                  padding: 5px 12px;
                  height: 100%;
                  overflow: hidden;
                  border-radius: 0;
                }
              }
            }
          }
        }
      }

      .rc-page04 {
        .rc-content-wrapper {
          padding: 0;
        }

        .rc-actor-car {
          width: 300px;
          height: 135px;
          right: -120px;
        }

        .rc-arrive {
          top: calc(50% - 67px);
          right: calc(50% - 150px);
        }
      }
    }
  }

  @keyframes rotate {
    from {
      transform: rotateZ(0deg);
    }

    to {
      transform: rotateZ(360deg);
    }
  }

  @-webkit-keyframes rotate {
    from {
      transform: rotateZ(0deg);
    }

    to {
      transform: rotateZ(360deg);
    }
  }

  @media screen and (min-width: 1368px) {
    .rc-cars-gallery {
      text-align: left;
    }
  }
</style>