<template>
  <div class="footer">
    <a href="https://home.baidu.com/" target="_blank">关于百度</a>
    <a href="https://ir.baidu.com/" target="_blank">About Baidu</a>
    <a href="https://www.baidu.com/duty/" target="_blank">使用百度前必读</a>
    <a href="https://help.baidu.com/question?prod_id=1" target="_blank"
      >帮助中心</a
    >
    <a href="https://e.baidu.com/?refer=1271" target="_blank">企业推广</a>
    <a
      href="https://beian.mps.gov.cn/#/query/webSearch?code=11000002000001"
      target="_blank"
      >京公网安备11000002000001号</a
    >
    <a href="https://beian.miit.gov.cn/" target="_blank">京ICP证030173号</a>
    <span>互联网新闻信息服务许可证：11220180008</span>
    <span>网络文化经营许可证：京网文 [2023] 1034-029号</span>
    <a href="https://www.baidu.com/licence/"
      >信息网络传播视听节目许可证 0110516</a
    >
    <div class="subjoin">
      <span
        ><el-icon><ArrowUp /></el-icon
      ></span>
      <div class="subjoin-div">
        <p>京ICP证030173号互联网新闻信息服务许可证11220180008</p>
        <p>网络文化经营许可证： 京网文〔2023〕1034-029号</p>
        <a href="https://www.baidu.com/licence/" target="_blank"
          >信息网络传播视听节目许可证 0110516</a
        >
        <p>互联网宗教信息服务许可证编号：京（2022）0000043</p>
        <p>药品医疗器械网络信息服务备案（京）网药械信息备字（2021）第00159号</p>
        <p>
          医疗器械网络交易服务第三方平台备案凭证（京）网械平台备字（2020）第00002号
        </p>
        <p>
          药品网络交易服务第三方平台备案凭证（京）网药平台备字〔2023〕第000002号
        </p>
        <p>©2024 Baidu</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ArrowUp } from "@element-plus/icons-vue";
</script>

<style scoped>
.footer {
  position: fixed;
  bottom: 1px;
  width: 99%;
  text-align: center;
  padding-bottom: 10px;
  background-color: inherit;
  font-size: 12px;
  padding-top: 5px;
}

.footer a {
  color: #bbb;
  text-decoration: none;
  padding: 5px;
}

.footer span {
  color: #bbb;
  padding: 5px;
}

.footer a:hover {
  color: #000;
}

.subjoin {
  display: inline-block;
  position: relative;
}

.subjoin-div {
  display: none;
  position: absolute;
  bottom: 100%;
  right: 0;
  width: 460px;
  height: auto;
  background-color: #fff;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.15);
  border-radius: 12px;
  z-index: 1000;
}

.subjoin-div a,
.subjoin-div p {
  display: flex;
  justify-content: flex-start;
  padding: 0 5px;
  margin: 4px 0;
  color: #626675;
  font-size: 12px;
}

.subjoin a {
  cursor: pointer;
}

.subjoin:hover .subjoin-div {
  display: block;
}

.subjoin span:hover {
  cursor: pointer;
}
</style>
