<template>
  <!--已抢到-->
  <div class="state-box">
    <div class="now">
      <ul>
        <li class="now-rob" v-for="item in take">
          <div class="wei-title">
            <img :src="item.shoesPicPath">
          </div>
          <div class="user-content">
            <div class="shang">
              <span class="shoesName" v-text="item.shoeName"></span>
              <span class="userName" v-text="item.userName"></span>
            </div>
            <div class="xiaa">
              <span class="shoesSize" v-text="'尺码:'+item.sizes"></span>
              <span class="shoesColor" v-text="'颜色:'+item.shoeDesc"></span>
            </div>
          </div>
          <div class="state-img" v-show="item.status == '4' || item.status == '0' || item.status == '1'">即将开抢</div>
          <div class="state-img" v-show="item.status == '5'">努力抢鞋中</div>
          <div class="state-img" v-show="item.status == '2'">
            <img src="/songbo/resources/static/img/cg3x.png">
          </div>
          <div class="state-img" v-show="item.status == '3'">
            <img src="/songbo/resources/static/img/sg3x.png">
          </div>
        </li>
      </ul>
    </div>
    <!--中间空白-->
    <!--抢鞋成功弹框-->
    <div class="success" v-show="succ">
      <div class="successMo">
        <div class="successmoTop">恭喜抢鞋成功!</div>
        <div class="successmoZhong"><a href="http://www.nike.com/">去支付</a></div>
        <div class="successmoBottom" @click.self="hidesuccess">取消</div>
      </div>
    </div>
  </div>
</template>

<style>
  /*整个背景*/
  .state-box{
    margin-top: 38px;
    z-index: 1;
  }
  .now{
    margin-top: 14px;
  }
  /*正在抢的每个人*/
  .now-rob {
    background-color: #FFFFFF;
    height: 61px;
    width: 100%;
    border-bottom: 1px solid rgb(229, 229, 229);
  }

  .wei-title, .user-content, .state-img {
    display: inline-block;
  }

  /*左*/
  .wei-title {
    width: 45px;
    height: 61px;
    line-height: 61px;
    border-radius: 50%;
  }

  .wei-title img {
    border-radius: 50%;
    width: 100%;
    height: 45px;
  }

  .user-content {
    height: 61px;
    margin-left: 8px;
  }

  /*中上*/
  .user-content .shang {
    display: flex;
    position: relative;
    top: 10px;
    font-size: 14px;
    color: #000000;
  }

  .shoesName {
    width: 120px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .userName {
    margin-left: 6px;
  }

  /*中下*/
  .now .xiaa {
    position: relative;
    top: 13px;
    font-size: 12px;
    color: #999999;
  }

  .shoesSize {
    margin-right: 6px;
  }
  /*右*/
  .state-img {
    position: absolute;
    right: 0;
    margin-right: 8px;
    height: 61px;
    line-height: 61px;
    font-size: 13px;
    color: #999999;
  }

  .state-img img {
    position: relative;
    right: 0;
    top: -1px;
    height: 50px;
    width: 50px;
  }

  /*抢鞋成功消息弹框*/
  .success {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.65);
    z-index: 56;
  }

  .successMo {
    background-color: #FFFFFF;
    border-radius: 12px;
    width: 246px;
    margin: 230px auto;
  }

  .successmoTop {
    text-align: center;
    font-size: 15px;
    color: #000000;
    font-weight: 700;
    padding: 28px 50px 21px 50px;
    border-bottom: 1px solid rgb(229, 229, 229);
  }

  .successmoZhong {
    border-bottom: 1px solid rgb(229, 229, 229);
  }

  .successmoZhong, .successmoBottom {
    height: 40px;
    line-height: 40px;
    text-align: center;
    font-size: 16px;
    color: #007AFF;
  }
a{
  color: #007AFF;
}
</style>

<script>
  export default{
    props: {
      second: {
        type: Number,
        default: 60
      }
    },
    data () {
      return {
        succ: false,
        take: [],
        now: false,
        fail: false,
        zhun: false,
        time: 0,
        state: "",
        inId: "",
        mess: []
      };
    },
    created: function () {
      var _this = this;
//      判断是否有user，没有就去短信绑定
      if (!user) {
        this.$router.push({path: '/login'});
      } else {
        var userId = user.id;
//        报名成功显示已抢到列表
        $.ajax({
          type: 'GET',
          url: url + '/Shoes/haveBuyShoes',
          data: {uid: userId},
          success: function (response) {
            if (response.error == 0) {
              _this.take = response.haveShoesDetail;
//            获取未读消息
              $.ajax({
                type: 'GET',
                url: url + '/Shoes/getMesgList',
                data:{type:1,uid: userId},
                success: function (response) {
                  if (response.error == 0) {
                    console.log("获取未读消息判断是否为空");
                    console.log(response.result);
                    _this.mess = response.result;
                    if (_this.mess.length > 0) {
                      _this.succ = true;
                    }
                  } else {
                    alert(response.error_mesg);
                  }
                }
              });
            } else {
              alert(response.error_mesg);
            }
          }
        });
      }

    },
    methods: {
//      隐藏支付成功模态框
      hidesuccess () {
        var _this = this;
        //            更新未读消息
        var messId = _this.mess[0].id;
        $.ajax({
          type: 'POST',
          url: url + '/Shoes/updateMesg',
          data: {id: messId},
          success: function (response) {
            if (response.error == 0) {
              _this.succ = false;
              _this.mess.shift();
//              读完一条消息过一秒再去检测是否还有未读消息
              var w = 1;
              var countdown0 = null;
              function timeShow0() {
                w--;
                if (w < 1) {
                  clearInterval(countdown0);
                  if (_this.mess.length > 0) {
                    _this.succ = true;
                  }
                }
              }
              countdown0 = setInterval(timeShow0, 1000);
            } else {
              alert(response.error_mesg);
            }
          }
        });
      }

    }

  };
</script>
