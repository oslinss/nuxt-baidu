<template>
  <div class="aside">
    <div class="help-div">
      <div class="help" @click="openHelp">
        <div class="help-more" >辅助模式</div>
      </div>
    </div>
    <div class="code-div">
      <div class="code">
        <div class="code-more">
          <div class="code-one">
            <div class="line-one">百度APP扫码登录</div>
            <div class="line-two">百度一下&nbsp;生活更好</div>
          </div>
          <img class="code-img"
            src="https://passport.baidu.com/v2/api/qrcode?sign=b15bb1849af167fb3c3c00f0cd0c410a&lp=pc&qrloginfrom=pc&logPage=traceId%3Apc_loginv5_1747572373%2ClogPage%3Aloginv5"
            alt="code"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['open-help']);
const openHelp = () => {
  emit('open-help'); // 触发open-help事件
};
</script>

<style scoped>
.aside {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: absolute;
  top: 520px;
  right: 20px;
  width: 50px;
  height: auto;
  background-color: inherit;
  border-radius: 20px;
  box-shadow: 0 2px 10px rgba(228, 223, 223, 0.15);
}

.help-div {
  height: 50px;
}

.code-div {
  height: 50px;
}

.help {
  width: 24px;
  height: 24px;
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAMAAABEpIrGAAAA7VBMVEUAAAD////////s8v////+txP+qwv+4zf/w9f/2+P+hu//Q3f+yyP+4zf/Q3f////+kvv+90P+80f+2yv/S4P/T4P/M2//z9/+cuP/V4P9Whv////9Uhf9Sg/9Pgf9NgP/8/f9di/9Xh/9lkf5aif9qlP7z9//k7P/c5v+2y/94nv51nP6lv/+LrP6Ep/6BpP5gjf7v9P+wxv/U4f/M2/+sxP+vxv73+f/P3v/J2v+5zf+ivP+fuv9xmf+Ytv6Usv6Hqf58of5vl/7m7v/g6f+zyf6QsP75+//q8P/B0v/W4//C1P6+0P6qwv6ct/76fHZiAAAAGnRSTlMAGAaVR/Py45aC9Mfy2b8t9OPZ2ce/v4L0x/e74/EAAAIZSURBVDjLZVPXYuIwEDSmQ4BLv5O0ku3Yhwu2IZTQe0hy7f8/57QSoYR5sVea1c424wgzl324LRRuH7I507hEJluYucCFEOBGhWzmy7X5+N0WwIjTbrcdBsKulM0z96onGCGE2X6n+cTkj/CqJ480igzkNXp26E9JkABSbBz8i4Bn3EkH840mKHoxs49fZQzt2Kd03FQEzSB3WsejB9Jqf1CJQBM0wCurABWBoub0gkDENwyStTHA62pwSWDtklRQ4FLfjnaiPqVW60hAYeLKNHIREOZuKTL80H6XBFCwn4BAmDOyLiOQUIlOSEjaoS+Ju57NZuul73Fml4w6yAivSLBW3MGfcfBmIegmArg3alICdJHgy1jQt8Z/6CcC4DdGXhLIoiWRACpbLYbDYW80GnXp2GH8ShP+PUvEoHsAIFq9Xm8+kXlIwkkI9pm+05Tm3yWqu9EiB0pkwjWBx2i+tND1XqeZqpPU4VhUbq/ekR8CwTRVoRxf3ifTbeIwcONNsJZ2lxFVKDMv1KNvS2zXdrnD+COvR1PQpTZKNlKD3cLCOJNnivgVxkw169BunlKFaV9/B+LQbqOsByY4IVgDB59dl/cjR9TIJV1Lh7CGqUqH/DDPhhZYOPkdLz6m0X7GrzPHsSe6zJwzxvm+5NeNi8U5ABfn7mz7zHJFrZ6+BY6rd7m8kQtcAtwwXzq4n69/vZbP1+pn6/8fsrRmHUhmpYYAAAAASUVORK5CYII=)
    no-repeat;
  background-size: 16px;
  background-position: center;
  border-radius: 50%;
  border: 13px solid transparent;
}

.code {
  width: 24px;
  height: 24px;
  margin: 0px auto;
  background-image: url(https://pss.bdstatic.com/static/superman/img/qrcode/qrcode@2x-daf987ad02.png);
  background-size: 24px;
  background-position: center;
  border: 13px solid inherit;
  border-radius: 50%;
}

.help:hover {
  width: 24px;
  height: 24px;
  margin: 0 auto;
  border: 10px solid inherit;
  border-radius: 50%;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.code:hover {
  width: 24px;
  height: 24px;
  margin: 0 auto;
  border-radius: 50%;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  background-image: url(https://pss.bdstatic.com/static/superman/img/qrcode/qrcode-hover@2x-f9b106a848.png);
  cursor: pointer;
}

.help-more {
  display: none;
  width: 80px;
  height: 30px;
  padding: 2px 5px;
  top: 9px;
  left: 0;
  right: auto;
  transform: translate(-107px, -3px);
  color: inherit;
  background-color: inherit;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.15);
  border-radius: 6px;
  z-index: 1000;
  font-size: 16px;
  line-height: 30px;
  text-align: center;
}

.help:hover .help-more {
  display: block;
}

.code-more {
  display: none;
  /* position: absolute; */
  width: 310px;
  height: 100px;
  padding: 2px 5px;
  /* top: 9px;
  left: 0;
  right: auto;
  transform: translateX(-100%); */
  transform: translate(-338px, -67px);
  background-color: inherit;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.15);
  border-radius: 6px;
  font-size: 16px;
  line-height: 30px;
  z-index: 1000;
}

.code-one {
  margin: 10px 20px 10px 5px;
}

.code-more .code-one .line-one {
  margin-right: 4px;
  font-size: 18px;
  color: inherit;
}

.code-more .code-one .line-two {
  margin-right: 4px;
  font: 10px;
  color: #9195a3;
}

.code-more .code-image {
  display: block;
  float: right;
}

.code:hover .code-more {
  display: block;
  display: flex;
}

.code-img {
  width: 80px;
  height: 80px;
  margin-top: 10px;
}
</style>
