<template>
  <!--手机号修改-->
  <div class="phone">
    <div class="phoneBox">
      <div class="telCon">
        <input type="tel" id="ytel" placeholder="请输入新手机号">
        <!--获取验证码按钮-->
        <div class="yan" @click="yan" :disabled="time > 0" v-text="text"></div>
      </div>
      <div class="telCon code">
        <input type="tel" id="ycode" placeholder="请输入验证码">
      </div>
    </div>
    <div class="exit" @click="sub2">
      完成修改
    </div>
  </div>
</template>

<style>
  .phone {
    background-color: #efeff4;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  .phoneBox {
    margin-top: 14px;
  }

  .telCon {
    align-items: center;
    background-color: #FFFFFF;
    border-bottom: 1px solid #e5e5e5;
    display: flex;
    justify-content: space-between;
    height: 56px;
    line-height: 56px;
    width: 100%;
    padding-left: 15px;
    padding-right: 15px;
  }

  #ytel,#ycode {
    height: 30px;
    outline: none;
    border: none;
  }

  #ytel::-webkit-input-placeholder {
    color: #cccccc;
  }

  #ycode::-webkit-input-placeholder {
    color: #cccccc;
  }
  /*获取验证码按钮*/
  .yan{
    position: relative;
    top: -5px;
    border-radius: 5px;
    margin-top: 13px;
    font-size: 12px;
    width: 70px;
    text-align: center;
    height: 30px;
    line-height: 30px;
    background-color: #ffcf0f;
    color: #FFFFFF;
  }
</style>

<script>
  export default{
    props: {
      cond: {
        type: Number,
        default: 60,
        sid: ""
      }
    },
    data () {
      return {
        type: "",
        time: 0
      }
    },
    created: function () {
      document.title = "手机号码";
    },
    methods: {
//      发送验证码方法
      timer() {
        if (this.time > 0) {
          this.time = this.time - 1;
          setTimeout(this.timer, 1000)
        }
      },
//    获取验证码
      yan() {
//      手机号
        var retrieveTel = $("#ytel").val();
        var codeType = this.type;
        var _this = this;
//      获取校验码
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
                      _this.time = _this.cond;
                      _this.sid = response.sid;
                      _this.timer();
                      _this.$emit('yan');
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
      //      完成修改
      sub2 () {
        var _this = this;
        var yTel = $("#ytel").val();
        var ycode = $("#ycode").val();
        if (yTel == "") {
          alert("手机号不能为空！");
        }else if (ycode == "") {
          alert("验证码不能为空！");
        } else {
          var yanPhone = user.phone;
            $.ajax({
              type: 'POST',
              url: url + '/login/updateUser',
              data: {phone: yanPhone,sid: _this.sid,newPhone: yTel,yzcode: ycode},
              beforeSend: function (request) {
                request.setRequestHeader("token", token);
              },
              success: function (response) {
                if (response.error == 0) {
                    console.log(response);
                  user = response.result.user;
                  console.log(user);
                  var tokens = response.result.token;
                  token = tokens.key;
                  _this.$router.push({path: '/meDe'});
                } else {
                  alert(response.error_mesg);
                }
              }
            });
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
