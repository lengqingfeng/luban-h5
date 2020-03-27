<template>
  <div class="container">
    <div class="titleStyle">鲁班H5</div>
    <a-card class="cardStyle">
      <a-form
        id="components-form-demo-normal-login"
        :form="form"
        class="login-form"
        @submit="handleSubmit"
      >
        <a-form-item>
          <a-input
            v-decorator="[
          'userName',
          { rules: [{ required: true, message: '请输入用户名！' }] },
        ]"
            placeholder="用户名"
          >
            <a-icon slot="prefix" type="user" style="color: rgba(0,0,0,.25)" />
          </a-input>
        </a-form-item>
        <a-form-item>
          <a-input
            v-decorator="[
          'password',
          { rules: [{ required: true, message: '请输入密码!' }] },
        ]"
            type="password"
            placeholder="密码"
          >
            <a-icon slot="prefix" type="lock" style="color: rgba(0,0,0,.25)" />
          </a-input>
        </a-form-item>
        <a-form-item>
          <a-checkbox
            v-decorator="[
          'remember',
          {
            valuePropName: 'checked',
            initialValue: true,
          },
        ]"
          >记住我</a-checkbox>
          <a class="login-form-forgot" href>忘记密码</a>
          <a-button type="primary" html-type="submit" class="login-form-button">登录</a-button>
        </a-form-item>
      </a-form>
    </a-card>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  beforeCreate() {
    this.form = this.$form.createForm(this, { name: "normal_login" });
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          let inputPassword = values["password"];
          let inputPhone = values["userName"];
          console.log("Received values of form: ", values);
          this.requestLogin(inputPhone, inputPassword);
        }
      });
    },
    requestLogin(phone, password) {
      console.log("start request");
      console.log("phone" + phone);
      console.log("password" + password);
      const that = this;
      axios({
        method: "post",
        url: "",
        data: {
          phone: phone,
          password: password,
          rememberMe: true,
          verifyDevice: false
        },
        headers: {
          'Content-Type':'application/json',
          'Accept':'application/json'
        }
      }).then(function(response) {
        console.log("success=========="+ response.data.access_token);
        console.log(response);
        localStorage.setItem('token', response.data.access_token);
         that.$router.push(`/`)
       }).catch(function(error) {
         console.log("error==========");
         console.log(error);
      });
    }
  },

  mounted() {
  }
};
</script>
<style>
.titleStyle {
  margin-top: 10%;
  text-align: center;
  height: 50px;
  color: white;
  line-height: 50px;
  font: bold;
  font-size: 30px;
}
.container {
  flex: 1;
  height: 100%;
  width: 100%;
  position: fixed;
  background-color: #2d3a4b;
}
.cardStyle {
  position: absolute;
  width: 300px;
  height: 260px;
  left: 50%;
  top: 50%;
  margin-left: -150px;
  margin-top: -130px;
}

#components-form-demo-normal-login .login-form {
  max-width: 300px;
}
#components-form-demo-normal-login .login-form-forgot {
  float: right;
}
#components-form-demo-normal-login .login-form-button {
  width: 100%;
}
</style>
