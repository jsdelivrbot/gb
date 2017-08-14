<template>
  <!--运动新闻-->
  <div class="sportNew">
    <!--运动轮播图-->
    <swiper class="swiper-box" :options="swiperOption">
      <swiper-slide class="swiper" v-for="(slide,index) in sportImg" :key="slide">
        <div class="banner-item" :style="'background-image: url('+slide.picPath+')'"><img src="../../assets/tou.png" @click="goImg(index)"></div>
      </swiper-slide>
      <div class="swiper-pagination" slot="pagination"></div>
    </swiper>
    <div class="lan"></div>
    <!--运动新闻内容-->
    <ul>
      <li class="sportList" :class="{'fen':sportitem.gbDairy.dairyType == 1}" v-for="sportitem in sportNew"
          @click="goDeta(sportitem.gbDairy.id,
              sportitem.gbDairy.uid,
              sportitem.gbDairy.title,
              sportitem.gbDairy.dairyType,
              sportitem.gbDairy.publishTime,
              sportitem.gbDairy.username,
              sportitem.gbDairy.description,
              1,
              sportitem.gbDairy.artReads,
              sportitem.gbDairy.reference
              )">
        <!--普通新闻-->
        <div class="sportCon" v-show="sportitem.gbDairy.dairyType == 1">
          <div class="sportCon-title" v-text="sportitem.gbDairy.title"></div>
          <div class="sportCon-play">
            <span class="sport-source" v-text="sportitem.gbDairy.reference"></span>
            <span class="sport-comment" v-text="sportitem.gbDairy.replys+'评论'"></span>
            <span class="sport-time" v-text="sportitem.gbDairy.publishTime"></span>
          </div>
        </div>
        <div class="sportImg" v-show="sportitem.gbDairy.dairyType == 1">
          <div class="banner-item" :style="'background-image: url('+sportitem.gbDairy.picPath+')'"><img src="../../assets/newList.png" ></div>
        </div>
        <!--GB话题-->
        <div class="gbtitle" v-show="sportitem.gbDairy.dairyType == 2" v-text="sportitem.gbDairy.title"></div>
        <div class="trends-img" v-show="sportitem.gbDairy.dairyType == 2">
          <ul class="list-imgss">
            <li class="trend-img" v-for="img in sportitem.pics">
              <img :src="img.filePath">
            </li>
          </ul>
        </div>
        <div class="gbplay" v-show="sportitem.gbDairy.dairyType == 2">
          <span class="gbsource">GB话题</span>
          <span class="sport-comment" v-text="sportitem.gbDairy.replys+'评论'"></span>
          <span class="sport-time" v-text="sportitem.gbDairy.publishTime"></span>
        </div>
      </li>
    </ul>
    <infinite-loading :class="{'hideLoad':noLoad == 1}" :on-infinite="onInfinite"
                      ref="infiniteLoading"></infinite-loading>
  </div>
</template>

<style>
  .hideLoad {
    clear: both;
    text-align: center;
    display: none;
  }
  .banner-item{
    background-position: center;
    background-size: cover;
  }
</style>

<script>
  import {swiper, swiperSlide, swiperPlugins} from 'vue-awesome-swiper';
  import InfiniteLoading from 'vue-infinite-loading';
  var moment = require('moment');
  moment.locale('zh-cn');

  export default{
    data () {
      return {
        noLoad: 0,
        topStatus: '',
        sportNew: [],
        sportImg: [],
        swiperOption: {
          loop: true,
          autoplay: 3000,                       //自动播放间隔时间
          setWrapperSize: true,
          pagination: '.swiper-pagination',
          paginationClickable: true,            //点击某一个圆点跳转到相应图片
          autoplayDisableOnInteraction: false   //操作之后会继续自动滑动
        },
        pageInt: 1
      };
    },
    created: function () {
      var _this = this;
      //    时间戳转换日期
      function times() {
        _this.sportNew.map((v) => {
          return (v.gbDairy.publishTime = moment(Number(v.gbDairy.publishTime)).fromNow());
        });
      }

//      运动新闻轮播图
      $.ajax({
        type: 'GET',
        url: url + '/artcle/carouselPicsList',
        data: {cartId: 1},
        success: function (response) {
          if (response.error == 0) {
            _this.sportImg = response.result;
          } else {
            alert(response.error_mesg);
          }
        }
      });
    },
    methods: {
//        点击某一条封面图
      goImg(index) {
        var imgUrl = this.sportImg[index].urlPic;
        window.location.href = imgUrl;
      },
//      点击某一条新闻进入详情页
      goDeta (newId, newuId, newTitle, newUrl, newTime, newName, newDesc, num, readys,reference) {
        this.$router.push({path: '/detalis/' + newId + '/' + newuId + '/' + newTitle + '/' + newUrl + '/' + newTime + '/' + newName + '/' + newDesc + '/' + num + '/' + readys + '/' + reference});
      },
      // 运动新闻上拉加载
      onInfinite() {
        function times1(sportObj) {
          sportObj.map((v) => {
            return (v.gbDairy.publishTime = moment(Number(v.gbDairy.publishTime)).fromNow());
          });
        }

        var _this = this;
        $.ajax({
          type: 'GET',
          url: url + '/artcle/artcleList',
          data: {cartId: 1, page: _this.pageInt},
          success: function (response) {
            if (response.error == 0) {
              var sd = response.result;
              _this.sportNew = _this.sportNew.concat(sd);
              times1(sd);
              if (sd.length == 0) {
                _this.noLoad = 1;
              }
              _this.pageInt = _this.pageInt + 1;
              _this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded');
            } else {
              alert(response.error_mesg);
            }
          }
        });
      }
    },
    components: {
      InfiniteLoading,
      swiper,
      swiperSlide
    }

  };
</script>
