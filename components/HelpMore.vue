<template>
  <div
    class="help-more-div"
    id="helpMoreDiv"
  >
    <div class="help-wrap">
      <div class="read-set">
        <img
          src="https://www.baidu.com/wza/public/theme/1/images/zh-CN1.png?domain=www.baidu.com&v=4.6.29"
        />
      </div>
      <div class="help-main">
        <div class="help-content">
          <div class="help-txt">读屏专用</div>
          <div class="help-btn">
            <a
              href="#"
              title="读屏专用：快捷键Ctrl+Alt+R，适用于使用读屏软件的用户，若无读屏软件可通过Ctrl+Alt+V开启声音获取语音提示"
            >
              <img
                class="help-img"
                src="https://s21.ax1x.com/2024/12/29/pAxDeyt.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/02/pEpC2SP.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">声音开关</div>
          <div class="help-btn">
            <a href="#" title="当前声音已关闭，开启声音Ctrl+Alt+V">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2024/12/29/pAxDMTS.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/03/pEp8v26.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">语速</div>
          <div class="help-btn">
            <a href="#" title="当前语速为1倍，调节语速Ctrl+Alt+K">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2024/12/29/pAxD1YQ.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/08/pECEGYF.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">阅读方式</div>
          <div class="help-btn">
            <a href="#" title="当前为指读，切换阅读方式Ctrl+Alt+F">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2024/12/29/pAxDlFg.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/08/pECElwV.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content" :style="{ backgroundColor: moreBackgroundColor }">
          <div class="help-txt">配色</div>
          <div class="help-btn">
            <a
              v-for="(theme, index) in themes"
              :key="index"
              href="#"
              :title="`切换配色Ctrl+Alt+T,当前为 ${theme.name}，下一主题为 ${
                themes[(index + 1) % themes.length].name
              }`"
              class="color-nav"
              @click.prevent="changeTheme(index)"
            >
              <img
                :src="theme.imageUrl"
                alt="配色方案"
                :class="[
                  'help-img',
                  { 'img-hide': currentThemeIndex !== index },
                ]"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">放大</div>
          <div class="help-btn">
            <a href="#" title="当前网页缩放倍数为1.0倍,网页放大Ctrl+Alt+加号">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2024/12/29/pAxDuef.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/02/pEpCWy8.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">缩小</div>
          <div class="help-btn">
            <a href="#" title="当前网页缩放倍数为1.0倍,网页缩小Ctrl+Alt+减号">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2025/01/02/pESg0hV.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/03/pEp8j8x.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">鼠标样式</div>
          <div class="help-btn">
            <a href="#" title="当前大鼠标已关闭，开启大鼠标Ctrl+Alt+M">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2025/01/02/pESgdkq.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/03/pEp8L5R.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">十字线</div>
          <div class="help-btn">
            <a href="#" title="当前十字线已关闭，开启十字线Ctrl+Alt+N">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2025/01/02/pESgDpT.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/03/pEpGprD.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">大字幕</div>
          <div class="help-btn">
            <a href="#" title="当前大字幕已关闭，开启大字幕请按Ctrl+Alt+B">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2024/12/29/pAxDdTU.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/02/pEpCRQf.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">重置</div>
          <div class="help-btn">
            <a href="#" title="重新设置Ctrl+Alt+C">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2024/12/29/pAxDawT.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/02/pEpCcWt.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">固定</div>
          <div class="help-btn">
            <a href="#" title="固定或者取消固定工具栏，请按Ctrl+Alt+L">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2025/01/02/pESgU7n.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/02/pEpCfOS.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt">说明</div>
          <div class="help-btn">
            <a href="#" title="开启操作说明Ctrl+Alt+问号键">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2025/01/02/pESgwt0.png"
                alt=""
              />
              <img
                src="https://s21.ax1x.com/2025/01/03/pEp8XP1.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
        <div class="help-content">
          <div class="help-txt" @click="closeHelp">退出服务</div>
          <div class="help-close">
            <a href="#" title="退出服务Ctrl+Alt+E" @click="closeHelp">
              <img
                class="help-img"
                src="https://s21.ax1x.com/2025/01/02/pESgscF.png"
                alt=""
                @click="closeHelp"
              />
              <img
                src="https://s21.ax1x.com/2025/01/08/pECEQe0.png"
                alt=""
                class="img-hide"
              />
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['close-help']);

const closeHelp = () => {
  emit('close-help');
};
</script>

<style scoped>
.help-more-div {
  display: flex;
  position: fixed;
  width: 100%;
  top: 0;
  height: 100px;
  background-color: #505050;
  z-index: 1001;
}

.help-more:focus .help-more-div {
  display: block;
}

.help-wrap {
  display: flex;
  justify-content: space-evenly;
  padding-top: 10px;
  padding-left: 80px;
}

.read-set {
  padding-top: 10px;
}

.help-main {
  display: flex;
  justify-content: flex-start;
  width: 80%;
}

.help-content {
  height: 93px;
  margin: 2px;
}

.help-btn img:focus {
  background-color: rgb(220, 46, 46);
}

.help-txt {
  font-size: 15px;
  color: #fff;
  text-align: center;
}

.help-btn img,
.help-close img {
  height: 48px;
  border-radius: 8px;
  margin-top: 10px;
}

.img-hide {
  display: none;
}

.help-btn img:hover {
  background-color: rgb(220, 46, 46);
}

.theme-default {
  background-color: #ffffff !important;
  color: #000000 !important;
}

.theme-dark-blue {
  background-color: #00294a !important;
  color: #ffffff !important;
}

.theme-chocolate {
  background-color: hsl(14, 100%, 17%) !important;
  color: #ffffff !important;
}

.theme-black-white {
  background-color: #000000 !important;
  color: #ffffff !important;
}

.theme-forest-green {
  background-color: #1b6f1b !important;
  color: #ffffff !important;
}

/* 交互样式 */
.help-btn img:hover,
.help-btn img:focus {
  background-color: rgb(220, 46, 46);
  outline: 2px solid #fff;
}

.help-txt {
  font-size: 15px;
  color: #fff;
  text-align: center;
  user-select: none;
}
</style>
