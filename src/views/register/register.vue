<template>
  <body>
  <div>
    <form action="" class="login">
      <p>注册</p>

      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="用户名称" prop="name">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" prop="email">
          <el-input v-model="ruleForm.email"></el-input>
        </el-form-item>
        <el-form-item label="年龄" prop="age">
          <el-input v-model.number="ruleForm.age"></el-input>
        </el-form-item>
        <el-form-item label="性别" prop="sex">
          <el-radio-group v-model="ruleForm.sex">
            <el-radio :label=2>女</el-radio>
            <el-radio :label=1>男</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="生日" required>
          <el-col :span="11">
            <el-form-item prop="date1" id="birthday">
              <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date1" style="width: 100%;"></el-date-picker>
            </el-form-item>
          </el-col>
        </el-form-item>
        <el-form-item label="密码" prop="pass">
          <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="checkPass">
          <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="验证码验证" prop="checkCode" @click="Code">
          <el-input type="text" v-model="ruleForm.checkCode" autocomplete="off" placeholder="点击下方获得验证码"></el-input>
          <div id="code" @click="check" ></div>
        </el-form-item>
        <el-form-item>
          <el-button id="button3" type="primary" plain @click="submitForm('ruleForm')">邮箱验证</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
          <el-button @click="Tologin">返回</el-button>
        </el-form-item>
      </el-form>
    </form>
  </div>
  </body>
</template>

<script>
import {judgeUsername, emailCheckHelp} from "@/api/api";

export default {
  name: "register",
  data() {
    var checkAge = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('年龄不能为空'));
      }
      setTimeout(() => {
        if (!Number.isInteger(value)) {
          callback(new Error('请输入数字值'));
        } else {
          callback();
        }
      }, 1000);
    };
    var validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'));
      } else {
        if (this.ruleForm.checkPass !== '') {
          this.$refs.ruleForm.validateField('checkPass');
        }
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'));
      } else if (value !== this.ruleForm.pass) {
        callback(new Error('两次输入密码不一致!'));
      } else {
        callback();
      }
    };
    var codeCheck = (rule, value, callback) => {
      var a = document.getElementById("code").innerText;
      if (value === '') {
        callback(new Error('请输入验证码'));
      } else if (value !== a) {
        callback(new Error('验证码不正确!'));
      } else {
        callback();
      }
    };
    return {
      ruleForm: {
        name: '',
        email: '',
        age: '',
        sex: '',
        date1:'',
        pass: '',
        checkPass: '',
        checkCode:'',
      },
      rules: {
        name: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        email: [
          { required: true, message: '请输入邮箱地址', trigger: 'blur' },
          { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }
        ],
        sex: [
          { required: true, message: '请选择性别', trigger: 'change' }
        ],
        date1: [
          { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
        ],
        pass: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { validator: validatePass, trigger: 'blur' }
        ],
        checkPass: [
          { required: true, message: '请再次输入密码', trigger: 'blur' },
          { validator: validatePass2, trigger: 'blur' }
        ],
        checkCode: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          { validator: codeCheck, trigger: 'blur' }
        ],
        age: [
          { required: true, message: '年龄不能为空', trigger: 'blur' },
          { validator: checkAge, trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(async (valid) => {
        if (valid) {
          const code = await judgeUsername(this.ruleForm.name);
          if (code.result === "no") {
            alert('用户名已被使用!')
          }
          else if (code.result === "yes"){
            alert('submit!');
            await emailCheckHelp(this.ruleForm.email)
            await this.$router.push({
              name: 'email',
              params: {
                username: this.ruleForm.name,
                userInfo: {
                  username: this.ruleForm.name,
                  password: this.ruleForm.pass,
                  email   : this.ruleForm.email,
                  age     : this.ruleForm.age,
                  gender  : this.ruleForm.sex
                }
              }
            });
          }
          else {
            alert("错误！");
          }
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    Tologin() {
      this.$router.push('/login');
    },
    check(){
      var codeStr = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
      var oDiv = document.getElementById('code');
// 用来生成随机整数
      function getRandom(n, m) { // param: (Number, Number)
        n = Number(n);
        m = Number(m);
        // 确保 m 始终大于 n
        if (n > m) {
          var temp = n;
          n = m;
          m = temp;
        }
        // 下有详细说明
        return Math.floor(Math.random()*(m - n) + n);
      }
// 将随机生成的整数下标对应的字母放入div中
      function getCode() {
        var str = '';
        // 验证码有几位就循环几次
        for (var i = 0;i < 4;i ++) {
          var ran = getRandom(0, 62);
          str += codeStr.charAt(ran);
        }
        oDiv.innerHTML = str;
      }
      getCode();// 调用函数，页面刷新也会刷新验证码
// 点击刷新验证码
      oDiv.onclick = function(){
        getCode();
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
  background: #353b65 url("../../assets/cloud.png") no-repeat;
  background-size: 120%;
  min-height: 400px;
  margin-top: -70px;
  margin-left: -10px;
  height:940px;
  width:1700px;
}
#button3{
  margin-left: -13px;
}
#birthday{
  width:150px;
}
#code{
  width: 80px;
  height: 38px;
  font-size: 18px;
  line-height: 40px;
  text-align: center;
  color: black;
  border: 1px solid red;
  margin-top: 5px;
  float:left;
  cursor: pointer;
}

#yui{
  margin-left: -470px;
  width: min-content;
  height: min-content;
}
.ti{
  margin-left: -160px;
  margin-top: 8px;
  width: max-content;
  height: max-content;
}
.login{
  position: absolute;
  top: 50%;
  margin-top: -360px;
  left: 50%;
  margin-left: -200px;
  /* absolute居中的一种方法 */
  background-color: rgba(255,255,255,0.7);
  width: 430px;
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
  background-color: whitesmoke;
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
  margin-top: 37px;
  font-size: 31px;
  font-weight: 600;
  color: white;
  float:left;
}
.btn1
{
  margin-left: 40px;
}
.btn:hover{
  background-color:#17D3AD
}
</style>
