<template>
  <div class="overlay" v-if="isAccountLoginActive || isMessageLoginActive"></div>
  <div
    class="login"
    id="toggleLogin"
    :style="{ backgroundColor: loginBackgroundColor }"
  >
    <div class="login-main">
      <div class="login-left">
        <div class="pass-form-logo">
          <img
            src="https://passport.baidu.com/passApi/img/newloginlogo.png"
            alt="baidu-logo"
          />
        </div>
        <div class="scan">
          <div class="scan-item">
            <div class="scan-left">
              <img
                src="https://passport.baidu.com/v2/api/qrcode?sign=0526…aceId%3Apc_loginv5_1738147570%2ClogPage%3Aloginv5"
                alt=""
                class="scan-img"
                width="150"
                height="150"
              />
            </div>
            <div class="scan-right">
              <img
                src="https://passport.baidu.com/passApi/img/qrcodeLoginGuide1.png"
                alt=""
                class="login-img"
                width="125"
                height="150"
              />
            </div>
          </div>
          <span class="scan-help">请使用百度APP扫码登录</span>
          <div class="download">
            <!-- <img
              src="https://ppui-static-pc.cdn.bcebos.com/passApi/img/baidu-download-icon.png"
            /> -->
            <img src="../assets/img/baidu.png" alt="">
            <a href="https://mo.baidu.com/wuxian/?from=pass">下载百度APP</a>
            <div class="download-main">
              <div class="download-main-item">
                <img
                  src="https://passport.baidu.com/v2/api/qrcode?sign=0526…aceId%3Apc_loginv5_1738147570%2ClogPage%3Aloginv5"
                  width="200"
                  height="200"
                  alt="download-img"
                />
              </div>
              <div class="download-app">扫描二维码下载百度APP</div>
            </div>
          </div>
        </div>
      </div>
      <div class="login-right">
        <div class="login-title">
          <div
            class="account-login"
            :class="{ active: isAccountLoginActive }"
            @click="showAccountLogin"
          >
          
            账号登录
          </div>
          <div
            class="message-login"
            @click="showMessageLogin"
          :class="{ active: isMessageLoginActive }"
          >
          
            短信登录
          </div>
        </div>
        <div class="account-login-plat"  v-if="isAccountLoginActive">
          <form
            class="account-form"
            method="POST"
            autocomplete="off"
          >
            <div class="message">
              <input
                v-model="accountUsername"
                type="text"
                name="userName"
                autocomplete="off"
                placeholder="手机号/用户名/邮箱"
                class="message-input"
              />
              
            </div>
            <div class="account-password">
              <input
                v-model="accountPassword"
                type="password"
                name="password"
                autocomplete="off"
                placeholder="密码"
                class="password-input"
                @input="handleCodeInput"
              />
            </div>
            <div class="hint">
              <div v-if="accountUsernameError" class="error-message">
                {{ accountUsernameError }}
              </div>
              <div v-if="accountPasswordError" class="error-message">
                {{ accountPasswordError }}
              </div>
            </div>
            <a
              class="forgot-password"
              href="https://passport.baidu.com/?getpassindex&tt=1735402646268&gid=CCA2619-1B17-49D2-9C07-73C657CA5A54&tpl=mn&u=https%3A%2F%2Fwww.baidu.com%2F"
              target="_blank"
              >忘记密码？</a
            >
            <div class="submit">
              <button
                type="submit"
                value="登录"
                class="submit-join"
                :style="{ opacity: isAgree ? 1 : 0.5 }"
                @click.prevent="validateAccountForm"
              >
                登录
              </button>
              <div class="agreement">
                <input
                  name="NameAgree"
                  id="IdAgree"
                  type="checkbox"
                  class="checkbox"
                  autocomplete="off"
                  v-model="isAgree"
                />
                <label for="IdAgree">阅读并接受</label>
                <a
                  target="_blank"
                  href="http://passport.baidu.com/static/passpc-account/html/protocal.html"
                  >百度用户协议</a
                >
                和
                <a
                  target="_blank"
                  href="https://privacy.baidu.com/policy/PrivacyPolicy"
                  >隐私政策</a
                >
              </div>
            </div>
          </form>
        </div>
        <div class="verification" v-if="isMessageLoginActive">
          <p class="creation">验证即登录，未注册将自动创建百度账号</p>
          <form
            action=""
            class="verification-platform"
          >
            <div class="verification-message">
              <input
                v-model="phone"
                type="text"
                name="username"
                placeholder="请输入手机号"
                class="verification-input"
              />
            </div>
            <div class="verification-check">
              <input
                v-model="verificationCode"
                type="text"
                name="password"
                placeholder="验证码"
                class="check-input"
                maxlength="6"
                @input="handleCodeInput"
                ref="codeInput"
              />
              <button class="clear-icon" @click.prevent="clearVerification">
                ×
              </button>
              <span class="white">|</span>
              <button
                class="verification-code"
                :style="{
                  cursor:
                    isCountdownActive || !isPhoneValid
                      ? 'not-allowed'
                      : 'pointer',
                  opacity: isCountdownActive || !isPhoneValid ? 0.7 : 1,
                }"
                @click.prevent="sendVerificationCode"
              >
                {{ countdownButtonText }}
              </button>
            </div>
            <div class="explain">
              <div v-if="showVerificationHint" class="error-message">
                收不到短信验证码？
              </div>
              <div v-if="verificationError" class="error-message">
                {{ verificationError }}
              </div>
              <div v-if="phoneEmpty" class="error-message">
                {{ phoneEmpty }}
              </div>
              <div v-if="phoneError" class="error-message">
                {{ phoneError }}
              </div>
              <div v-if="agreeError" class="error-message">
                {{ agreeError }}
              </div>
            </div>
            <div class="message-submit">
              <button
                type="submit"
                value="登录"
                class="message-button"
                :style="{ opacity: isAgree ? 1 : 0.5 }"
                @click.prevent="handleLogin"
              >
                登录
              </button>
              <div class="message-agreement">
                <input
                  name="message-Agree"
                  id="MessageAgree"
                  type="checkbox"
                  class="message-checkbox"
                  autocomplete="off"
                  v-model="isAgree"
                  @change="agreeError = ''"
                />
                <label for="IdAgree">阅读并接受</label>
                <a
                  target="_blank"
                  href="http://passport.baidu.com/static/passpc-account/html/protocal.html"
                  >百度用户协议</a
                >
                和
                <a
                  target="_blank"
                  href="https://privacy.baidu.com/policy/PrivacyPolicy"
                  >隐私政策</a
                >
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
    <div class="login-foot">
      <div class="other-way">
        <a
          href="https://graph.qq.com/oauth2.0/show?which=Login&display=pc&client_id=100312028&response_type=code&redirect_uri=https%3A%2F%2Fpassport.baidu.com%2Fphoenix%2Faccount%2Fafterauth%3Fmkey%3D98c2867b5366e327962af6517e2dc46cb2c7424b63eae5a725%26tpl%3Dmn&state=1738146052&display=page&scope=get_user_info%2Cadd_share%2Cget_other_info%2Cget_fanslist%2Cget_idollist%2Cadd_idol%2Cget_simple_userinfo&traceid="
          target="_blank"
          ><img src="https://passport.baidu.com/passApi/img/bd-acc-qzone.png"
        /></a>
        <a
          href="https://api.weibo.com/oauth2/authorize?client_id=2512457640&response_type=code&redirect_uri=https%3A%2F%2Fpassport.baidu.com%2Fphoenix%2Faccount%2Fafterauth%3Fmkey%3Daea74ab61b988f78e3dc3e15f7cbc7b83de731b0c692aa45c1%26tpl%3Dmn&forcelogin=1&only_qr=1&state=1738146247&display=page&traceid=###"
          target="_blank"
          ><img src="https://passport.baidu.com/passApi/img/bd-acc-tsina.png"
        /></a>
        <a
          href="https://open.weixin.qq.com/connect/qrconnect?appid=wx85f17c29f3e648bf&response_type=code&scope=snsapi_login&redirect_uri=https%3A%2F%2Fpassport.baidu.com%2Fphoenix%2Faccount%2Fafterauth%3Fmkey%3D8a3ce6e07c64a99681770daf06338cf9e3168fcdc2224592f9%26tpl%3Dmn%26appid%3Dwx85f17c29f3e648bf%26traceid%3D&state=1738146285&display=page&traceid="
          target="_blank"
          ><img src="https://passport.baidu.com/passApi/img/bd-acc-weixin.png"
        /></a>
      </div>
      <div class="login-now-way">
        <a
          class="login-now"
          href="https://passport.baidu.com/v2/?reg&tt=1735402646268&overseas=undefined&gid=CCA2619-1B17-49D2-9C07-73C657CA5A54&tpl=mn&u=https%3A%2F%2Fwww.baidu.com%2F"
          target="_blank"
          >立即注册</a
        >
      </div>
      <NuxtLink to="/">
        <div class="close-btn" @click="closeLogin">
        <img
          src="https://passport.baidu.com/passApi/img/uni-close.png"
          width="20"
          height="20"
        />
      </div>
      </NuxtLink>
      
    </div>
  </div>
</template>


<script setup name="Login" lang="js">
import {computed, ref} from 'vue'

const emit=defineEmits();

function closeLogin() {
  // 控制登录页面弹窗关闭
  emit('close')
}

const isAccountLoginActive = ref(true); // 账号登录是否激活
const isMessageLoginActive = ref(false); // 短信登录是否激活

function showAccountLogin() {
  isAccountLoginActive.value = true;
  isMessageLoginActive.value = false;
}

function showMessageLogin() {
  isAccountLoginActive.value = false;
  isMessageLoginActive.value = true;
}

const accountUsername = ref(""); // 账号登录用户名
const accountPassword = ref(""); // 账号登录密码
const phone = ref(""); // 手机号
const verificationCode = ref(""); // 验证码
const verificationError = ref(""); // 存储验证码错误信息
const phoneEmpty = ref(""); // 存储手机号错误信息
const phoneError = ref(""); // 存储手机号错误信息
const agreeError = ref(""); // 存储协议错误信息
const isAgree = ref(false); // 是否同意协议
const countdown = ref(0); // 倒计时
const isCountdownActive = ref(false); // 是否正在倒计时
const showVerificationHint = ref(false); // 是否显示“收不到短信验证码”
const timer = ref(null); // 保存计时器引用
const accountUsernameError = ref(""); // 账号登录用户名错误信息
const accountPasswordError = ref(""); // 账号登录密码错误信息

const countdownButtonText = computed(() => {
  if (!isCountdownActive.value) {
    return "发送验证码"; // 倒计时未开始时显示“发送验证码”
  } else if (countdown.value > 0) {
    return `${countdown.value}s`; // 倒计时中显示剩余时间
  } else {
    return "重新发送"; // 倒计时结束后显示“重新发送”
  }
});

function validateAccountForm() {
  // 账号登录校验
  if(!accountUsername.value.trim()&&isAgree.value){
    accountUsernameError.value = "请您输入手机号/用户名/邮箱";
    accountPasswordError.value = false;
    return;
  }
  if(!accountPassword.value.trim()&&isAgree.value){
    accountPasswordError.value = "请输入密码";
    accountUsernameError.value = false;
    return;
  }
}

function handleLogin() {
  if (!phone.value.trim()&&isAgree.value) {
    // 检查手机号是否为空
    phoneEmpty.value = "请输入手机号码";
    phoneError.value=false;
  } else if (!/^1[3-9]\d{9}$/.test(phone.value.trim())&&isAgree.value) {
    phoneError.value = "手机号码格式不正确";
    phoneEmpty.value=false;
  } else if(verificationCode.value.trim() === ""&&isAgree.value){
    verificationError.value = "请填写验证码";
    phoneEmpty.value=false;
    phoneError.value=false;
  }
}

function sendVerificationCode() {
  if(!isAgree.value){
    agreeError.value = "请阅读并接受相关协议";
    return;
  }
  if(phone.value.trim()=== "") {
    phoneEmpty="请输写手机号"
  }
  if(verificationCode.value.trim() !== ""){
    showVerificationHint.value = false; // 当验证码输入时隐藏提示
  }
  // 开始倒计时
  isCountdownActive.value = true; 
  countdown.value = 60;
  timer.value = setInterval(() => {
    countdown.value--;
    if (countdown.value <= 0 ) {
      clearInterval(timer.value);
      showVerificationHint.value = false; // 倒计时结束后重置提示
    }
    if (countdown.value <= 49 && verificationCode.value.trim() === "") {
      showVerificationHint.value = true; // 显示收不到短信验证码提示
      verificationError.value=false;
      agreeError.value = ""; // 重置协议错误提示
    }
  }, 1000);
}

function handleCodeInput() {
  verificationError.value = "";
  if (verificationCode.value.trim() !== "") {
    showVerificationHint.value = false; // 当验证码输入时隐藏提示
  }
  else if(accountPassword.value.trim() !== ""){
    accountPasswordError.value = false;
  }
}

function clearVerification() {
  verificationCode.value = "";
}
</script>


<style>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); 
  z-index: 1112; 
}

.login {
  display: none;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 800px;
  height: 425px;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  border-radius: 10px;
  box-shadow: 0 4px 20px 460px rgba(0, 0, 0, 0.1);
  z-index: 1113;
}

.login-main {
  display: flex;
  margin-top: 30px;
}

.close-btn {
  cursor: pointer;
  position: absolute;
  top: 20px;
  right: 20px;
}

.pass-form-logo {
  display: flex;
  width: auto;
  height: 30px;
  margin: -5px 0 20px 30px;
}

.login-left {
  margin-left: 18px;
  padding-right: 37px;
  border-right: 1px solid #f5f5f6;
}

.login-right {
  float: right;
  margin-left: 15px;
}

.scan {
  margin-top: 10px;
  margin-left: 15px;
}

.scan-item {
  display: flex;
  justify-content: space-evenly;
  margin-left: 10px;
  margin-top: 35px;
  margin-bottom: 20px;
}

.scan-left {
  display: inline-block;
  border: 1px solid rgb(133, 133, 133, 0.3);
  border-radius: 6px;
  margin-right: 33px;
}

.scan-right {
  display: inline-block;
}

.login-foot {
  display: flex;
  justify-content: space-between;
  border-top: 1px solid rgb(133, 133, 133, 0.3);
  margin-top: 19px;
}

.other-way {
  padding-top: 10px;
  margin-bottom: 10px;
  margin-left: 30px;
}

.other-way img {
  width: 30px;
  height: 30px;
  padding: 5px;
}

.scan-help {
  font-size: 19px;
  padding-left: 75px;
}

.download {
  display: flex;
  width: 150px;
  height: 35px;
  background-color: #f5f5f6;
  margin-top: 12px;
  margin-left: 98px;
  border-radius: 5px;
}

.download img {
  width: 20px;
  height: 20px;
  background-color: #f4f9f9;
  border-radius: 5px;
  padding-left: 10px;
  padding-top: 9px;
}

.download a {
  font-size: 14px;
  padding-left: 10px;
  padding-top: 9px;
  color: #4e6ef2;
  text-decoration: none;
}

.download > img:hover,
.download:hover a {
  opacity: 50%;
}

.login-now {
  display: inline-block;
  margin-top: 17px;
  margin-left: 520px;
  font-size: 14px;
  color: #4e6ef2;
  text-decoration: none;
}

.login-title {
  display: flex;
  flex-direction: row;
  font-size: 18px;
  color: #9195a3;
  margin-left: -9px;
}

.account-login,
.message-login {
  margin: 10px 20px 5px 20px;
  cursor: pointer;
}

.account-login.active,
.message-login.active {
  color: #000;
  border-bottom: 3px solid #4e6ef2;
}

.login-title div {
  margin: 0 20px;
  padding-bottom: 5px;
}
.account-login-plat {
  margin-top: 17px;
}

.message input,
.account-password input {
  width: 330px;
  height: 45px;
  border: 1px solid #9195a3;
  border-radius: 8px;
  margin: 10px;
  font-size: 17px;
  padding-left: 20px;
}

.forgot-password {
  display: flex;
  justify-content: flex-end;
  color: #4e6ef2;
  text-decoration: none;
  font-size: 14px;
  margin-top: 4px;
  margin-right: 20px;
}

.submit input[type="submit"],
.message-submit input[type="submit"] {
  width: 330px;
  height: 50px;
  margin: 10px 25px 5px 25px;
  background-color: #4e6ef2;
  border-radius: 10px;
  border: 0;
  box-shadow: 0 6px 16px 0 rgb(78 111 242 / 30%);
  font-size: 15px;
  color: #fff;
  opacity: 50%;
  cursor: pointer;
}

.agreement,
.message-agreement {
  margin: 10px 0 5px 50px;
  font-size: 14px;
  color: #525252;
}

.agreement a,
.message-agreement a {
  color: #4e6ef2;
  text-decoration: none;
}

.download-main {
  display: none;
  flex-direction: column;
  position: absolute;
  width: 350px;
  height: 250px;
  background: #fff;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.15);
  border-radius: 12px;
  bottom: 133px;
  left: 35px;
}

.download-main img {
  width: 150px;
  height: 150px;
  margin-left: 100px;
  margin-top: 30px;
  border: 1px solid #bbb;
  background-color: #fff;
  padding: 0;
}

.download-app {
  margin-top: 20px;
  margin-left: 82px;
  font-size: 18px;
}

.download:hover .download-main {
  display: flex;
}

.message-login:active .creation {
  color: #626675;
}

#accountLogin {
  display: none;
}

#messageLogin {
  display: none;
}

.creation {
  font-size: 12px;
  color: #858585;
  margin-left: 10px;
}

.verification-message input[type="text"] {
  width: 330px;
  height: 45px;
  border: 1px solid #9195a3;
  border-radius: 8px;
  margin: 10px;
  font-size: 17px;
  padding-left: 20px;
}

.verification-check {
  display: flex;
  width: 354px;
  height: 47px;
  border-radius: 8px;
  margin: 10px;
  font-size: 17px;
  border: 1px solid #9195a3;
}

.verification-code {
  background-color: #fff;
  border: none;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  color: #4e6ef2;
  font-size: 16px;
}

.verification-check input[type="text"] {
  border: none;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
  padding-left: 20px;
  font-size: 17px;
}

.verification-check input:focus {
  outline: none;
}

.agreement input:focus .submit-join {
  color: #395ff8;
}

.message-submit {
  margin-top: -10px;
}

.message input:focus,
.account-password input:focus,
.verification-message input[type="text"]:focus {
  border-color: #4e6ef2;
  outline: none;
}

.verification-check:focus .verification-check input {
  border-color: #4e6ef2;
  outline: none;
}

.explain {
  height: 21px;
}

.hint {
  display: flex;
  height: 21px;
}

.clear-icon {
  background-color: inherit;
  border: none;
  padding-top: 5px;
  margin-left: -44px;
}

.white {
  background-color: inherit;
  padding-top: 11px;
  color: gray;
  padding-left: 45px;
}

.verification-code:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.error-message {
  color: #ff4444;
  font-size: 12px;
  margin-left: 15px;
  margin-top: 5px;
  height: 20px;
}

/* 调整输入框错误状态 */
.message-input:invalid,
.password-input:invalid,
.verification-input:invalid {
  border-color: #ff4444;
}

.submit-join,
.message-button {
  width: 330px;
  height: 50px;
  margin: 10px 25px 5px 25px;
  background-color: #4e6ef2;
  border-radius: 10px;
  border: 0;
  box-shadow: 0 6px 16px 0 rgb(78 111 242 / 30%);
  font-size: 15px;
  color: #fff;
  opacity: 50%;
  cursor: pointer;
}

.account-form {
  display: block;
  margin-top: -10px;
}
</style>
