<template>
  <!--注册第二个页面-->
  <div class="register">
    <div class="register-box">
      <!--头像 -->
      <div>
        <div class="headImg">
          <img src="../../assets/登录-logo@2x.png">
        </div>
        <form>
          <input type="file" name="file" accept="image/*" id="upload"/>
        </form>
        <div class="head-txt">
          添加头像
        </div>
      </div>
      <!--设置昵称-->
      <div class="nick">
        <div class="nick-icon">
          <img src="../../assets/login2x.png">
        </div>
        <div class="nick-input">
          <input type="text" class="nick-val" name="nick" placeholder="设置昵称">
        </div>
      </div>
      <!--选择性别-->
      <div class="sex">
        <div class="sex-icon">
          <img src="../../assets/zcnv2x.png">
        </div>
        <ul>
          <li class="sex-list" @click="man">
            <div class="man">
              <img :src="manImg"/>
              <span class="man-txt">男</span>
            </div>
          </li>
          <li class="sex-list" @click="woman">
            <div class="woman">
              <img :src="womanImg"/>
              <span class="woman-txt">女</span>
            </div>
          </li>
          <li class="sex-list" @click="confide">
            <div class="confide">
              <img :src="confideImg"/>
              <span class="confide-txt">保密</span>
            </div>
          </li>
        </ul>
      </div>
      <!--完成按钮-->
      <div class="over" @click="over">完成</div>
    </div>
  </div>
</template>

<style>
  * {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .register-box {
    padding: 40px 60px;
  }

  /*头像*/
  .headImg {
    margin: 0 auto;
    width: 109px;
    height: 109px;
    border-radius: 50%;
  }

  .headImg img {
    border-radius: 50%;
    width: 100%;
    height: 100%;
  }

  #upload {
    margin: 10px auto;
    width: 4em;
    height: 20px;
    opacity: 0;
  }

  .head-txt {
    position: relative;
    top: -30px;
    cursor: pointer;
    pointer-events: none;
    text-align: center;
    margin-top: 9px;
    font-size: 14px;
    color: #999999;
  }

  /*设置昵称*/
  .nick {
    border-bottom: 1px solid #e5e5e5;
    margin-top: 15px;
    display: flex;
    padding-left: 5px;
  }

  .nick-icon {
    position: relative;
    top: -2px;
    width: 20px;
    height: 20px;
  }

  .nick-icon img {
    width: 100%;
    height: 100%;
  }

  .nick-input {
    flex: 1;
    margin-left: 10px;
  }

  .nick-val {
    width: 100%;
    border: none;
    outline: none;
    font-size: 14px;
    color: #000000;
  }

  .nick-val::-webkit-input-placeholder {
    color: #cccccc;
  }

  /*性别*/
  .sex {
    display: flex;
    margin-top: 30px;
    padding-left: 6px;
  }

  .sex-icon {
    width: 20px;
    height: 20px;
  }

  .sex-icon img {
    width: 100%;
    height: 100%;
  }

  .sex-list {
    float: left;
    margin-left: 28px;
    font-size: 0px;
    color: #000000;
  }

  .sex-list img {
    width: 14px;
    height: 14px;
    position: relative;
    top: -5px;
    right: 8px;
  }

  .confide-txt, .woman-txt, .man-txt {
    font-size: 14px;
  }
</style>

<script>
  export default{
    data () {
      return {
        sex: "",
        manImg: "/songbo/resources/static/img/xznv2x.png",
        womanImg: "/songbo/resources/static/img/wxnv2x.png",
        confideImg: "/songbo/resources/static/img/wxnv2x.png",
        sex: "",
        tel: "",
        code: "",
        pass: ""
      }
    },
    created: function () {
        document.title = "注册";
//        接收参数,手机号／验证码／密码
      this.tel = this.$route.params.tel;
      this.code = this.$route.params.code;
      this.pass = this.$route.params.pass;
    },
    mounted () {
      //        点击添加图像
      var upload = document.querySelector('#upload');
      var pvc = document.querySelector('.headImg');
      upload.addEventListener('change', function (e) {
        pvc.innerHTML = '<img src=' + (window.URL.createObjectURL(this.files[0])) + '>';
        var formData = new FormData();
        formData.append("file",this.files[0]);
//        上传头像
        $.ajax({
          type: 'POST',
          url: url + '/upload',
          data: formData,
          contentType: false,
          processData: false,
          success: function (response) {
            if (response.error == 0) {
              TitleImg = response.urlList[0];
            }else {
              alert(response.error_mesg);
            }
          }
        });
      }, false);
      //        选择性别
      $(".sex-list").click(function () {
        var sexIndex = $(this).index();
        if (sexIndex == 0) {
          this.sex = $(".man-txt").text();
        }
        if (sexIndex == 1) {
          this.sex = $(".woman-txt").text();
        }
        if (sexIndex == 2) {
          this.sex = $(".confide-txt").text();
        }
      });

    },
    methods: {
      man()
      {
        this.manImg = "/songbo/resources/static/img/xznv2x.png";
        this.womanImg = "/songbo/resources/static/img/wxnv2x.png";
        this.confideImg = "/songbo/resources/static/img/wxnv2x.png";
        this.sex = "1";
      }
      ,
      woman()
      {
        this.manImg = "/songbo/resources/static/img/wxnv2x.png";
        this.womanImg = "/songbo/resources/static/img/xznv2x.png";
        this.confideImg = "/songbo/resources/static/img/wxnv2x.png";
        this.sex = "2";
      }
      ,
      confide()
      {
        this.manImg = "/songbo/resources/static/img/wxnv2x.png";
        this.womanImg = "/songbo/resources/static/img/wxnv2x.png";
        this.confideImg = "/songbo/resources/static/img/xznv2x.png";
        this.sex = "0";
      }
      ,
      //        完成
      over()
      {
        var _this = this;
        var nickname = $(".nick-val").val();
//        注册请求
        $.ajax({
          type: 'POST',
          url: url + '/login/register',
          data: {
            phone: _this.tel,
            yzcode: _this.code,
            password: _this.pass,
            userName: nickname,
            gender: _this.sex,
            headPicPath: TitleImg
          },
          success: function (response) {
            if (response.error = 0) {
              user = response.user;
              _this.$router.push({path: '/login'});
            } else {
              alert(response.error_mesg);
            }
          }
        });
      }
    }
  }
  ;
</script>
