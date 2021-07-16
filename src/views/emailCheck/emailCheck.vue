<template>
  <body>
  <div class="backgroud">
    <form action="" class="login">
      <p>CSI语音技术吧</p>
      <p id="warn">&nbsp;</p>
      <input type="text" v-model="checkCode" placeholder="邮箱验证">
      <br>
      <br>
      <el-button type="submit" class="btn btn1" @click="toEmailCheck"> 验   证 </el-button>
    </form>
  </div>
  </body>
</template>

<script>
import {register} from "@/api/api";

export default {
  name: "emailCheck",
  data() {
    return {
      username: '',
      userInfo: {},
      checkCode: ''
    }
  },

  created() {
    this.username = this.$route.params.username;
    this.userInfo = this.$route.params.userInfo;
  },
  methods: {
    async toEmailCheck() {
      const judge = await register(this.userInfo, this.checkCode);
      if (judge.code === "yes") {
        alert("注册成功！")
        await this.$router.push('/login');
      }
      else if (judge.code === "no") {
        alert("注册失败，验证码错误！");
      }
      else {
        alert("出错！");
      }
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
  background: #353b65 url("../../assets/blue.jpg") no-repeat;
  background-size: 100%;
  min-height: 279px;
  margin-top: -100px;
  margin-left: -20px;
  margin-right: -25px;
  height: 920px;
  width:2000px;
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
  margin-left: 87px;
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