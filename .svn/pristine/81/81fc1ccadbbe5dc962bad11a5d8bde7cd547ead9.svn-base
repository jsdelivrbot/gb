<template>
  <!--修改密码-->
  <div class="pass">
    <div class="passBox">
      <div class="passList">
        <div class="passTitle">当前密码</div>
        <div class="passInput">
          <input type="password" placeholder="请输入" id="oldPass">
        </div>
      </div>
      <div class="passList">
        <div class="passTitle">新密码</div>
        <div class="passInput">
          <input type="password" placeholder="请输入" id="newPass">
        </div>
      </div>
      <div class="passList">
        <div class="passTitle">确认密码</div>
        <div class="passInput">
          <input type="password" placeholder="请输入" id="quePass">
        </div>
      </div>
      <div class="exit" @click="modpass">
        确定
      </div>
    </div>
    <div class="countTi">
      此账号仅用于Nike官网抢鞋
    </div>
  </div>
</template>

<style>
  /*整个背景*/
  .pass {
    background-color: #efeff4;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  .passBox {
    background-color: #FFFFFF;
    margin-top: 14px;
  }

  /*每一条*/
  .passList {
    display: flex;
    height: 56px;
    line-height: 56px;
    width: 100%;
    padding-left: 20px;
    padding-right: 20px;
    border-bottom: 1px solid #e5e5e5;
  }

  /*前面*/
  .passTitle {
    font-size: 16px;
    width: 4em;
    flex-shrink: 0;
    margin-right: 20px;
  }

  /*右边*/
  .passInput {
    width: 100%;
    align-items: center;
    font-size: 16px;
    color: #000000;
  }

  .passInput input {
    line-height: normal;
    width: 100%;
    height: 30px;
    font-size: 16px;
    border: none;
    outline: none;
  }

  #oldPass::-webkit-input-placeholder {
    color: #cccccc;
  }

  #newPass::-webkit-input-placeholder {
    color: #cccccc;
  }

  #quePass::-webkit-input-placeholder {
    color: #cccccc;
  }
</style>

<script>
  export default{
    created: function () {
      document.title = "修改密码";
    },
    methods: {
//        提交修改代码
      modpass () {
        var _this = this;
        var oldPass = $("#oldPass").val();
        var newPass = $("#newPass").val();
        var quePass = $("#quePass").val();
        var yanPhone = user.phone;
        if (oldPass == "") {
          alert("当前密码不能为空！");
        } else if (newPass == "") {
          alert("新密码不能为空！");
        } else if (quePass == "") {
          alert("确认密码不能为空！");
        }else if (newPass != quePass) {
          alert("两次密码不一样！");
        }else {
          $.ajax({
            type: 'POST',
            url: url + '/login/updateUser',
            data: {phone: yanPhone,oldPassword: oldPass,password: quePass},
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
    }
  }
</script>
