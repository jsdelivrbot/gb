<template>
  <!--我-->
  <div class="meBox">
    <!--头部-->
    <div class="meTop">
      <div class="mo" v-show="back"
           :style="userImg === '/songbo/resources/static/img/meTitle2x.png' ?'' : 'background-image:url('+ userImg +')'"></div>
      <div class="meCon">我</div>
      <div class="meTitle">
        <img :src="userImg" @click="meDe">
      </div>
      <div class="meLogin" @click="goLogin" v-text="meText" :class="{'bor': nameBor == 1}"></div>
    </div>
    <!--收藏／官网账号-->
    <div class="meTow">
      <div class="meKuai ws" @click="collect">
        <div class="meLeft">
          <span class="meLeftImg">
          <img src="../../assets/wdeshoucang2x.png">
          </span>
          <span class="meLeft-con">我的收藏</span>
        </div>
        <div class="meRight">
          <img src="../../assets/mejian2x.png">
        </div>
      </div>
      <div class="meKuai" @click="count">
        <div class="meLeft">
          <span class="meLeftImg">
          <img src="../../assets/mebang2x.png">
          </span>
          <span class="meLeft-con">Nike官网账号</span>
        </div>
        <div class="meRight">
          <img src="../../assets/mejian2x.png">
        </div>
      </div>
    </div>
    <!--意见反馈-->
    <div class="meKuai kuai2" @click="feed">
      <div class="meLeft">
        <span class="meLeftImg">
        <img src="../../assets/meyi2x.png">
        </span>
        <span class="meLeft-con">意见反馈</span>
      </div>
      <div class="meRight">
        <img src="../../assets/mejian2x.png">
      </div>
    </div>
    <!--关于GB-->
    <div class="meKuai kuai2" @click="about">
      <div class="meLeft">
        <span class="meLeftImg">
          <img src="../../assets/woguanyu.png">
        </span>
        <span class="meLeft-con">关于GB</span>
      </div>
      <div class="meRight">
        <img src="../../assets/mejian2x.png">
      </div>
    </div>
  </div>
</template>

<style>
  .meBox {
    position: fixed;
    height: 100%;
    width: 100%;
    top: 0;
    left: 0;
    background-color: #efeff4;
  }

  .mo {
    background-color: rgba(0, 0, 0, 0.6);
    filter: blur(28px);
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    transform: scale(1.6);
  }

  /*头部*/
  .meTop {
    overflow: hidden;
    position: relative;
    background-color: #ffcf0f;
    width: 100%;
    padding-top: 30px;
    padding-bottom: 2px;

  }

  .meCon {
    position: relative;
    z-index: 1;
    text-align: center;
    font-size: 18px;
    color: #ffffff;
  }

  /*头像*/
  .meTitle {
    position: relative;
    border: 3px solid #ffffff;
    margin: 20px auto 0;
    width: 77px;
    height: 77px;
    border-radius: 50%;
    z-index: 1;
  }

  .meTitle img {
    border-radius: 50%;
    width: 100%;
    height: 100%;
  }

  /*登录按钮*/
  .meLogin {
    margin: 12px auto;
    width: 100%;
    text-align: center;
    height: 28px;
    line-height: 25px;
    font-size: 14px;
    color: #ffffff;
    position: relative;
    z-index: 1;
  }

  .bor {
    border: 2px solid #ffffff;
    border-radius: 2px;
    margin: 12px auto;
    width: 83px;
    text-align: center;
    height: 28px;
    line-height: 25px;
    font-size: 14px;
    color: #ffffff;
    position: relative;
    z-index: 1;
  }

  /*第二块*/
  .meTow, .kuai2 {
    margin-top: 10px;
  }

  /*每一条*/
  .meKuai {
    box-sizing: border-box;
    padding: 0 15px 0 20px;
    display: flex;
    justify-content: space-between;
    background-color: #ffffff;
    width: 100%;
    height: 56px;
    line-height: 56px;
  }
.ws{
  border-bottom: 1px solid #e5e5e5;
}
  /*左侧*/
  .meLeft {
    height: 56px;
    line-height: 56px;
    font-size: 0;
  }

  .meLeftImg {
    display: inline-block;
    width: 25px;
    height: 56px;
    line-height: 56px;
  }

  .meLeftImg img {
    margin-top: -11px;
    width: 25px;
  }

  .meLeft-con {
    margin-left: 15px;
    font-size: 16px;
  }

  /*右侧箭头*/
  .meRight img {
    width: 15px;
    height: 15px;
  }
</style>

<script>
  export default{
    data () {
      return {
        back: false,
        nameBor: 1,
        userImg: "/songbo/resources/static/img/meTitle2x.png",
        meText: "登录／注册"
      }
    },
    created: function () {
        document.title = "";
//      判断是否登录过
      if (user) {
        this.meText = user.nickname;
        if (user.headPicPath) {
          this.userImg = user.headPicPath;
//      模糊背景显示
          this.back = true;
        } else {
          this.userImg = "/songbo/resources/static/img/meTitle2x.png";
        }
//      显示昵称没有边框
        this.nameBor = 0;
      } else {
        this.userImg = wxTitle;
        this.meText = wxName;
//        模糊背景
        this.back = true;
//        显示昵称没有边框
        this.nameBor = 0;
      }
    },
    methods: {
//        我的信息
      meDe () {
        if (!user) {
          this.$router.push({path: '/login'});
        } else {
          this.$router.push({path: '/meDe'});
        }
      },
//      登录注册
      goLogin () {
        if (!user) {
          this.$router.push({path: '/login'});
        } else {
          this.$router.push({path: '/meDe'});
        }
      },
//      意见反馈
      feed () {
        if (!user) {
          this.$router.push({path: '/login'});
        } else {
          this.$router.push({path: '/feed'});
        }
      },
//      官网账号
      count () {
        if (!user) {
          this.$router.push({path: '/login'});
        } else {
          this.$router.push({path: '/count'});
        }
      },
//      我的收藏
      collect () {
        if (!user) {
          this.$router.push({path: '/login'});
        } else {
          this.$router.push({path: '/collect'});
        }
      },
//      关于GB
      about () {
        this.$router.push({path: '/about'});
      }
    }
  }
</script>
