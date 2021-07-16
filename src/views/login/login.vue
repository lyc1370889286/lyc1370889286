<template>
  <body>
  <div class="background">
    <form action="" class="login">
      <p>CSI语音技术吧</p>
      <p id="warn">&nbsp;</p>
      <input type="text" v-model="userName" placeholder="用户名">
      <input type="password" v-model="userPassword" placeholder="密码">
      <br>
      <br>
      <a  class="btn2" href="/camera" > 人脸识别 </a>
      <el-button type="submit" class="btn" @click="toLogin"> 登  录 </el-button>
      <el-button type="submit" class="btn btn1" @click="toRegister"> 注  册 </el-button>
    </form>
  </div>
  </body>
</template>

<script>
// import {requestPost, requestGet} from "@/api/api";
import {login} from "@/api/api";

export default {
  name: "login",
  data() {
    return {
      msg: 'null',
      userName: '',
      userPassword: ''
    }
  },
  methods: {
    async toLogin() {
      let userInfo = {"username": this.userName, "password": this.userPassword};
      let judge = await login(userInfo);
      console.log(judge);
      console.log((judge.token))
      if (judge.result === "yes") {
        window.sessionStorage.setItem("token", judge.token);
        alert("登陆成功")
        await this.$router.push('/menu');
      } else {
        console.log("failed");
      }
    },

    toRegister() {
      this.$router.push('/register');
    }
  }
}
</script>

<style scoped>
*{
  user-select: none;
  /* 无法选中，整体感更强 */
}
body{
  background: #353b65 url("../../assets/img.png") no-repeat;
  background-size: 120%;
  min-height: 400px;
  margin-top: -90px;
  margin-left: -10px;
  height:1000px;
  width:1700px;
}
#yu{
  margin-left: -544px;
  width: min-content;
  height: min-content;
}
.login{
  position: absolute;
  top: 50%;
  margin-top: -200px;
  left: 50%;
  margin-left: -200px;
  /* absolute居中的一种方法 */
  background-color: rgba(255,255,255,0.7);
  width: 400px;
  height: 400px;
  border-radius: 25px;
  text-align: center;
  padding: 5px 40px;
  box-sizing: border-box;
  /* 这样padding就不会影响大小 */
}
p{
  font-size: 42px;
  font-weight: 600;
}

input{
  background-color: rgba(0,0,0,0);
  width: 100%;
  height: 40px;
  margin-bottom: 10px;
  border: none;
  border-bottom: 2px solid silver;
  /* 下面的会覆盖上面的步伐 */
  outline: none;
  font-size: 22px;
}

.btn{
  display: block;
  background-color:#3399CC ;
  width: 42%;
  height: 52px;
  border-radius: 23px;
  margin-top: 30px;
  font-size: 31px;
  color: rgba(255,255,255,0.6);
  float:left;
}
.btn1
{
  margin-left: 40px;
  margin-top: 30px;
}
.btn2
{
  margin-top:10px;
  margin-left:-560px;
}
.btn:hover{
  background-color:#17D3AD;
  color: rgba(255,255,255,1);
}
#warn{
  font-size: small;
  color:red;
}
</style>