<template>
  <!--找回密码／注册-->
  <div class="retrieve">
    <div class="retrieve-box">
      <!--手机号-->
      <div class="retrieve-list">
        <div class="retrieve-tel-icon">
          <img class="retrieve-img" src="../../assets/zs2x.png">
        </div>
        <div class="input">
          <input type="tel" class="input-val" name="retrieve-tel" placeholder="请输入手机号码" v-model="retrieveTel">
        </div>
        <!--获取验证码按钮-->
        <div class="obtain" :class="{'obtainColor':changeObtain == 1}" @click="send" :disabled="time > 0"
             v-text="text"></div>
      </div>
      <!--验证码-->
      <div class="retrieve-list">
        <div class="retrieve-code-icon">
          <img class="retrieve-img" src="../../assets/yan2x.png">
        </div>
        <div class="input">
          <input type="number" class="input-val" name="retrieve-code" placeholder="请输入手机验证码" v-model="retrieveCode">
        </div>
      </div>
      <!--新密码-->
      <div class="retrieve-list">
        <div class="retrieve-password-icon">
          <img class="retrieve-img" src="../../assets/dm2x.png">
        </div>
        <div class="input">
          <input type="password" class="input-val" name="retrieve-password" placeholder="设置6-12位新密码"
                 v-model="retrievePassword">
        </div>
      </div>
      <!--重复新密码-->
      <div class="retrieve-list">
        <div class="retrieve-password-icon">
          <img class="retrieve-img" src="../../assets/dm2x.png">
        </div>
        <div class="input">
          <input type="password" class="input-val" name="retrieve-password" placeholder="重复新密码"
                 v-model="retrievePassword2">
        </div>
      </div>
      <!--完成按钮-->
      <div class="over">
        <div v-if="type == 0" @click="next">下一步</div>
        <div v-else @click="retrieve">完成</div>
      </div>
    </div>
  </div>
</template>

<style>
  * {
    margin: 0;
    padding: 0;
  }

  .retrieve-box {
    padding: 12px 60px;
  }

  .retrieve-list {
    display: flex;
    border-bottom: 1px solid #e5e5e5;
    margin-top: 38px;
    padding-left: 5px;
  }

  .retrieve-list:first-child {
    margin-top: 26px;
  }

  .input {
    flex: 1;
    margin-left: 10px;
  }

  .input-val {
    border: none;
    outline: none;
    font-size: 14px;
    color: #000000;
  }
  .input-val::-webkit-input-placeholder {
    color: #cccccc;
  }
  .retrieve-img {
    width: 100%;
    height: 100%;
  }

  /*手机*/
  .retrieve-tel-icon {
    position: relative;
    top: -2px;
    width: 12px;
    height: 15px;
  }

  .obtain {
    position: absolute;
    margin-top: -4px;
    right: 60px;
    width: 70px;
    text-align: center;
    height: 21px;
    line-height: 21px;
    font-size: 12px;
    color: #ffffff;
    background-color: #ffcf0f;
    border-radius: 3px;
  }

  /*验证码*/
  .retrieve-code-icon {
    position: relative;
    top: -2px;
    width: 14px;
    height: 14px;
  }

  .obtainColor {
    background-color: #999999;
  }

  /*密码*/
  .retrieve-password-icon {
    position: relative;
    top: -2px;
    width: 14px;
    height: 16px;
  }

  /*完成*/
  .over {
    border: 1px solid #ffcf0f;
    border-radius: 5px;
    margin-top: 36px;
    height: 37px;
    line-height: 37px;
    text-align: center;
    font-size: 14px;
    color: #ffcf0f;
  }

  .over a {
    text-decoration: none;
    color: #ffcf0f;
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
        type: "",
        changeObtain: "",
        retrieveTel: "",
        retrieveCode: "",
        retrievePassword: "",
        retrievePassword2: "",
        time: 0
      }
    },
    created: function () {
//        接收参数类型，找回密码／注册
      this.type = this.$route.params.id;
    },
    methods: {
      //        发送验证码方法
      timer() {
        if (this.time > 0) {
          this.time = this.time - 1
          setTimeout(this.timer, 1000)
        }
      },
      send() {
        //          手机号／验证码类型
        var retrieveTel = this.retrieveTel;
        var codeType = this.type;
        var ds = this;
//          获取校验码
        if (retrieveTel == "") {
          alert("手机号码不能为空！")
        } else {
          $.ajax({
            type: 'GET',
            url: url + '/sms/gethash',
            data: {phone: retrieveTel},
            success: function (response) {
              if (response.error == 0) {
                var vcode = response.vcode;
//              获取验证码
                $.ajax({
                  type: 'POST',
                  url: url + '/sms/getsms',
                  data: {phone: retrieveTel, vcode: vcode, type: codeType},
                  success: function (response) {
                    if (response.error == 0) {
                      ds.time = ds.second;
                      ds.timer();
                      ds.$emit('send')
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
//      修改密码方法
      retrieve () {
//          手机号／验证码／密码／重复密码
        var retrieveTel = this.retrieveTel;
        var retrieveCode = this.retrieveCode;
        var retrievePassword = this.retrievePassword;
        var retrievePassword2 = this.retrievePassword2;
        var This = this;
        if (retrieveTel == "") {
          alert("手机号码不能为空！")
        } else if (retrieveCode == "") {
          alert("验证码不能为空！")
        } else if (retrievePassword == "") {
          alert("密码不能为空！")
        } else if (retrievePassword2 == "") {
          alert("请再次输入密码！")
        } else if (retrievePassword != retrievePassword2) {
          alert("两次密码不一致！")
        } else {
//          修改密码
          $.ajax({
            type: 'POST',
            url: url + '/login/resetpwd',
            data: {phone: retrieveTel, yzcode: retrieveCode, password: retrievePassword2},
            success: function (response) {
              if (response.error != 0) {
                alert(response.error_mesg);
              } else {
                alert("修改成功！");
                user = response.user;
                This.$router.replace({path: '/'});
              }
            }
          });
        }
      },
//      下一步
      next () {
//          手机号／验证码／密码／重复密码
        var retrieveTel = this.retrieveTel;
        var retrieveCode = this.retrieveCode;
        var retrievePassword = this.retrievePassword;
        var retrievePassword2 = this.retrievePassword2;
        var _this = this;
        if (retrieveTel == "") {
          alert("手机号码不能为空！")
        } else if (retrieveCode == "") {
          alert("验证码不能为空！")
        } else if (retrievePassword == "") {
          alert("密码不能为空！")
        } else if (retrievePassword2 == "") {
          alert("请再次输入密码！")
        } else if (retrievePassword != retrievePassword2) {
          alert("两次密码不一致！")
        } else {
          _this.$router.replace({path: '/register/' + retrieveTel + '/' + retrieveCode + '/' + retrievePassword});
        }
      }
    },
    computed: {
      text() {
        return this.time > 0 ? this.time + 's' : '获取验证码';
      }
    }
  };
</script>
