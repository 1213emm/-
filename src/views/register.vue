<template>
  <div id="login" class="login">
    <div class="wrap">
      <h1>注 册</h1>
      <el-form :model="form" ref="form" class="form">
        <el-form-item prop="username">
          <el-input placeholder="学号" v-model="form.id" autocomplete="off"></el-input>
        </el-form-item>
         <el-form-item prop="username">
          <el-input placeholder="用户名" v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="username">
          <el-input placeholder="密保问题" v-model="form.security_issue" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="username">
          <el-input placeholder="密保答案" v-model="form.security_answer" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="username">
          <el-input placeholder="密码" v-model="form.password1" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item id="password" prop="password">
          <el-input
              placeholder="确认密码"
              show-password
              type="password"
              v-model="form.password2"
              autocomplete="off"
              @keyup.enter.native="login"
          ></el-input>
        </el-form-item>
        <el-form-item class="btn_login">
          <el-button type="primary" @click="register">注&nbsp;&nbsp;册</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import qs from "qs";
export default {
  name: "NewRegister",
  data() {
    return {
      form: {
        id: '',
        username: '',
        security_issue:'',
        security_answer:''
      }
    }
  },
  methods: {
    register: function () {
      // 检查表单是否有填写内容
      if (this.form.id === '' || this.form.password1 === ''|| this.form.password2 === '' || this.form.username === '' || this.form.security_issue === '' || this.form.security_answer === '') {
        this.$message.warning("请填写完整信息!");
        return;
      }
      this.$axios({
        method: 'post',           
        url: '/api/user/register/',       
        data: qs.stringify({      
          id: this.form.id,
          username: this.form.username,
          password_1: this.form.password1,
          password_2: this.form.password2,
          security_issue: this.form.security_issue,
          security_answer: this.form.security_answer
        })
      })
      .then(res => {              /* res 是 response 的缩写 */
        switch (res.data.errno) {
          case 0:
            this.$message.success("注册成功！");
            setTimeout(() => {
                this.$router.push('/Login');
            }, 1000);
            break;
          case 1002:
            this.$message.error("该学号已被注册!");
            break;
          case 1003:
            this.$message.error("两次输入的密码不相同!");
            break;
        }
      })
      .catch(err => {
        console.log(err);         /* 若出现异常则在终端输出相关信息 */
      })
    }
  }
}
</script>

<style scoped>
#login {
  font-family: 'Noto Serif SC', serif;
  position:absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(246, 246, 246);
}
#login >>> .el-input__inner {
  font-family: 'Noto Serif SC', serif;
}
#login .bgbox {
  display: block;
  opacity: 1;
  z-index: -3;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: opacity 1s,transform .25s,filter .25s;
  backface-visibility: hidden;
}
#login .wrap {
  width: 350px;
  height: auto;
  padding: 0 25px 0 25px;
  line-height: 40px;
  position: relative;
  display: inline-block;
  background-color: rgba(255, 255, 255, 0.85);
  border-radius: 20px;
  margin-top: 50px;
  box-shadow: darkgrey 1px 1px 1px 1px ;
}
#login .btn_login {
  margin-top: 25px;
  text-align: center;
}
#login .btn_login button{
  line-height: 10px;
  font-family: 'Noto Serif SC', serif;
  width: 100%;
  height: 38px;
}
</style>