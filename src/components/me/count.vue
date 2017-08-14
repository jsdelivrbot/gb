<template>
  <!--官网账号-->
  <div class="count">
    <!--账号和密码-->
    <div class="countBox">
      <div class="countTop">
        <div class="countCon">官网邮箱</div>
        <input type="email" id="countEmail" placeholder="请输入" v-model="nikeaccount">
      </div>
      <div class="countTop">
        <div class="countCon">官网密码</div>
        <input type="password" id="countpass" placeholder="请输入" v-model="nikepassword">
      </div>
    </div>
    <div class="countTi">
      此账号仅用于Nike官网抢鞋
    </div>
    <!--确定按钮-->
    <div class="confrim" @click="confrim">
      确定
    </div>
    <!--账号验证-->
    <div class="valid-box" v-show="valid">
      <div class="valid">
        <div class="validImg">
          <img :src="validimg" :class="{'zhu': zhuan == 0}">
        </div>
        <div class="validIcon" v-text="validIcon"></div>
      </div>
    </div>
  </div>
</template>

<style>
  /*整个背景*/
  .count {
    border-top: 1px solid #e5e5e5;
    background-color: #efeff4;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  /*包括两条的块*/
  .countBox {
    margin-top: 10px;
  }

  /*每一条*/
  .countTop {
    display: flex;
    background-color: #ffffff;
    border-bottom: 1px solid #e5e5e5;
    padding-left: 20px;
    padding-right: 20px;
    width: 100%;
    height: 56px;
    align-items: center;
  }

  .countCon {
    font-size: 16px;
    width: 4em;
    flex-shrink: 0;
    margin-right: 20px;
  }

  /*输入框*/
  #countEmail,#countpass {
    flex: 1;
    font-size: 16px;
    height: 30px;
    line-height: normal;
    color: #000000;
    border: none;
    outline: none;
  }
  #countEmail::-webkit-input-placeholder {
    color: #cccccc;
  }
  #countpass::-webkit-input-placeholder {
    color: #cccccc;
  }

  .countTi {
    height: 30px;
    line-height: 30px;
    width: 100%;
    text-align: center;
    font-size: 12px;
    color: #cccccc;
  }

  /*确定*/
  .confrim {
    position: fixed;
    bottom: 0;
    left: 0;
    height: 48px;
    line-height: 48px;
    width: 100%;
    text-align: center;
    background-color: #ffffff;
  }

  /*验证成功*/
  .valid-box {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 56;
  }

  .valid {
    padding-top: 17px;
    padding-bottom: 18px;
    margin: 230px auto;
    width: 152px;
    border-radius: 10px;
    background-color: rgba(0, 0, 0, 0.8);
  }

  .validImg {
    margin: 0 auto;
    width: 27px;
    height: 27px;
  }

  .validImg img {
    width: 100%;
    height: 100%;
  }

  .validIcon {
    width: 110px;
    text-align: center;
    margin: 11px auto 0;
    font-size: 16px;
    color: #FFFFFF;
  }
  .zhu {
    animation: rr 1s linear infinite;
    width: 100%;
    height: 100%;
  }

  @keyframes rr {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>

<script>
  export default{
    data () {
      return {
        zhuan: 0,
        valid: false,
        validimg: "",
        validIcon: "",
        nikeaccount: "",
        nikepassword: ""
      }
    },
    created: function () {
        document.title = "耐克官网账号";
      var _this = this;
      userId = user.id;
//        请求账号信息接口
        $.ajax({
          type: 'GET',
          url: url + '/Shoes/autoUserList',
          data: {uid: userId},
          success: function (response) {
            if (response.error == 0) {
              console.log(response);
              var infos = response.NikeUser;
              if (infos) {
                _this.nikeaccount = infos.nikeAccount;
                _this.nikepassword = infos.nikePassword;
              }else {
                _this.nikeaccount = "";
                _this.nikepassword = "";
              }
            } else {
              alert(response.error_mesg);
            }
          }
        });
    },
    methods: {
//     提交
      confrim () {
        var countEmail = $("#countEmail").val();
        var countpass = $("#countpass").val();
        if (countEmail == "") {
          alert("账号不能为空！")
        } else if (countpass == "") {
          alert("密码不能为空！")
        } else {
          var _this = this;
          _this.valid = true;
          _this.validimg = "/songbo/resources/static/img/jz2x.png";
          _this.validIcon = "正在验证账号...";
//        验证耐克官网账号
          $.ajax({
            type: 'GET',
            url: url + '/login/checkNikeAccount',
            data: {nikename: countEmail, nikepassword: countpass},
            success: function (response) {
              var errorCode = response.error;
              var i = 1;
              var countdown = null;
              function timeShow() {
                i--;
                if (i < 1) {
                  if (errorCode == 0) {
                    clearInterval(countdown);
                    _this.validimg = "/songbo/resources/static/img/tcg2x.png";
                    _this.validIcon = "账号验证成功";
                    _this.zhuan = "1";
                    var j = 1;
                    var countdown1 = null;

                    function timeShow1() {
                      j--;
                      if (j < 1) {
                        clearInterval(countdown1);
                        _this.valid = false;
//                        添加账号
                        $.ajax({
                          type: 'POST',
                          url: url + '/Shoes/saveNikeuser',
                          data: {uid: userId, nikeAccount: countEmail, nikePassword: countpass, phone: phone},
                          beforeSend: function (request) {
                            request.setRequestHeader("token", token);
                          },
                          success: function (response) {
                            if (response.error != 0) {
                              alert(response.error_mesg);
                            }
                          }
                        });
                      }
                    }
                    countdown1 = setInterval(timeShow1, 1000);
                  } else if (errorCode == 1) {
                    clearInterval(countdown);
                    console.log(response);
                    _this.validimg = "/songbo/resources/static/img/tsb2x.png";
                    _this.validIcon = "账号验证失败请重新输入";
                    _this.zhuan = "1";
                    var a = 1;
                    var countdown2 = null;

                    function timeShow2() {
                      a--;
                      if (a < 1) {
                        clearInterval(countdown2);
                        _this.valid = false;
                      }
                    }
                    countdown2 = setInterval(timeShow2, 1000);
                  }
                }
              }
              countdown = setInterval(timeShow, 1000);
            }
          });
        }
      }
    }
  };
</script>
