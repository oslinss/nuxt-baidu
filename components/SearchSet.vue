<template>
  <div class="overlay" v-if="isSearchSettingVisible || isAdvancedSearchVisible"></div>
  <div class="close-search-plat" @open-set="showSetting" :style="{backgroundColor: searchSetBackgroundColor}">
    <div class="close-search-header">
      <div class="header-left">
        <div class="search-set" :class="{ active: isSearchSettingVisible }"
        @click="showSearchSetting">搜索设置</div>
        <div class="high-search" :class="{ active: isAdvancedSearchVisible }"
        @click="showAdvancedSearch">高级搜索</div>
      </div>
      <div class="close-set" @click="closeSet">
        <NuxtLink to="/">
          <el-icon class="close-set-btn"><Close /></el-icon>
        </NuxtLink>
      </div>
    </div>
    <div v-if="isSearchSettingVisible" class="search-set-content" id="searchSet">
      <div class="set-items">
        <ul>
          <li>
            <span class="set-item"
              >搜索框提示：<span class="color-gray"
                >是否希望在搜索时显示搜索框提示</span
              ></span
            >
          </li>
          <li>
            <span class="set-item"
              >搜索语言范围：<span class="color-gray"
                >设定您所要搜索的网页内容的语言</span
              ></span
            >
          </li>
          <li>
            <span class="set-item"
              >搜索结果显示条数：<span class="color-gray"
                >设定您希望搜索结果显示的条数</span
              ></span
            >
          </li>
          <li>
            <span class="set-item"
              >实时预测功能：<span class="color-gray"
                >是否希望在您输入时实时展现搜索结果</span
              ></span
            >
          </li>
          <li>
            <span class="set-item"
              >搜索历史记录：<span class="color-gray"
                >是否希望在搜索时显示您的搜索历史
              </span></span
            >
          </li>
        </ul>
        <div class="set-choose">
          <form id="set-choose">
            <input type="radio" name="display" id="display" checked />
            <label for="display">显示</label>
            <input type="radio" name="display" id="no-display" />
            <label for="display">不显示</label>
          </form>
          <form id="set-choose">
            <input type="radio" name="lang" id="all" checked />
            <label for="all">全部语言</label>
            <input type="radio" name="lang" id="easy" />
            <label for="easy">仅简体中文</label>
            <input type="radio" name="lang" id="difficult" />
            <label for="difficult">仅繁体中文</label>
          </form>
          <form id="set-choose">
            <input type="radio" name="amount" id="ten" checked />
            <label for="ten">每页10条</label>
            <input type="radio" name="amount" id="twenty" />
            <label for="twenty">每页20条</label>
            <input type="radio" name="amount" id="thirty" />
            <label for="thirty">每页30条</label>
          </form>
          <form id="set-choose">
            <input type="radio" name="switch" id="open" checked />
            <label for="open">开启</label>
            <input type="radio" name="switch" id="close" />
            <label for="close">关闭</label>
          </form>
          <form id="set-choose">
            <input type="radio" name="search" id="display" checked />
            <label for="display">显示</label>
            <input type="radio" name="search" id="no-display" />
            <label for="no-display"
              >不显示<span>(该选项可以关闭您的历史记录) </span></label
            >
          </form>
          <form id="set-btn">
            <button class="refresh">恢复默认</button>
            <button class="save">保存设置</button>
          </form>
        </div>
      </div>
      <div class="close-search-footer">
        请启用浏览器cookie确保设置生效，<span
          class="login-way"
          >登录</span
        >后操作可永久保存您的设置。
      </div>
    </div>
    <div v-if="isAdvancedSearchVisible" class="high-search-content" id="highSearch">
      <ul>
        <li>
          <span>搜索结果：</span>
          <div class="key-word">
            <div class="key-left">
              <div>
                <span>包含全部关键词</span>
                <span class="color-gray">|</span>
                <input type="text" />
              </div>
              <div>
                <span>包含任意关键词</span>
                <span class="color-gray">|</span>
                <input type="text" />
              </div>
            </div>
            <div class="key-right">
              <div>
                <span>包含完整关键词</span>
                <span class="color-gray">|</span>
                <input type="text" />
              </div>
              <div>
                <span>不包含关键词</span>
                <span class="color-gray">|</span>
                <input type="text" />
              </div>
            </div>
          </div>
        </li>
        <li>
          <span
            >时间：<span class="color-gray"
              >限定要搜索的网页的时间是</span
            ></span
          >
          <select>
            <option selected="selected">时间不限</option>
            <option value="">一天内</option>
            <option value="">一周内</option>
            <option value="">一月内</option>
            <option value="">一年内</option>
          </select>
        </li>
        <li>
          <span>文档格式：<span class="color-gray">搜索网页格式是</span></span>
          <select>
            <option value="" selected="selected">所有网页和文件</option>
            <option value="">PDF(.pdf)</option>
            <option value="">Word(.doc)</option>
            <option value="">Excel(.xls)</option>
            <option value="">PowerPoint(.ppt)</option>
            <option value="">RTF(.rtf)</option>
            <option value="">所有格式</option>
          </select>
        </li>
        <li>
          <span
            >关键词位置：<span class="color-gray">查询关键词位于</span></span
          >
          <span id="way-choose">
            <input type="radio" name="way" id="anyway" checked />
            <label for="anyway">网页任何地方</label>
            <input type="radio" name="way" id="centerway" />
            <label for="centerway">仅网页标题中</label>
            <input type="radio" name="way" id="url" />
            <label for="url">仅URL中</label>
          </span>
        </li>
        <li>
          <span
            >站内搜索：<span class="color-gray"
              >限定要搜索指定的网站是</span
            ></span
          >
          <span class="web"><input type="text" /></span>
          <span class="color-gray">例如：baidu.com</span>
        </li>
      </ul>
      <form id="high-btn">
        <button>高级搜索</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { Close } from '@element-plus/icons-vue';

const isSearchSettingVisible = ref(true);
const isAdvancedSearchVisible = ref(false);

const showSearchSetting = () => {
  isSearchSettingVisible.value = true;
  isAdvancedSearchVisible.value = false;
};

const showAdvancedSearch = () => {
  isSearchSettingVisible.value = false;
  isAdvancedSearchVisible.value = true;
};

</script>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); 
  z-index: 1112; 
}
.close-search-plat {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 800px;
  height: 380px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 20px 460px rgba(0, 0, 0, 0.1);
  z-index: 1113;
  pointer-events: auto;
  color:#000;
}

.close-search-header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-left: 5px;
  margin-bottom: -10px;
}

.header-left {
  display: flex;
  flex-direction: row;
}

.header-left div {
  padding-bottom: 5px;
  margin: 20px;
}

.close-set-btn {
  display: block;
  width: 20px;
  height: 20px;
  cursor: pointer;
  padding: 10px;
  color: gray;
}

.close-set-btn:hover {
  color: #4e6ef2;
}

.search-set.active,
.high-search.active {
  color: #000;
  border-bottom: 3px solid #4e6ef2;
}

.set-items {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin-left: -63px;
}

.set-item {
  font-size: 13px;
  color: inherit;
}

.search-set-content ul {
  list-style-type: none;
}

.search-set-content li {
  padding-bottom: 20px;
}

.set-choose {
  display: flex;
  flex-direction: column;
  margin-top: 19px;
}

#set-choose {
  display: inline-block;
  font-size: 13px;
  padding-right: 20px;
  padding-bottom: 23px;
}

#set-btn button {
  border: none;
  border-radius: 6px;
  padding: 5px 12px;
  cursor: pointer;
}

#set-btn button:hover {
  background-color: #4662d9;
}

.refresh {
  color: #000;
  background-color: #f5f5f6;
  margin-right: 10px;
}

.refresh:hover {
  color: #fff;
}

.save {
  color: #fff;
  background-color: #4e6ef2;
}

.close-search-footer {
  font-size: 13px;
  color: #9195a3;
  padding: 30px 23px;
}

.login-way {
  color: #4e6ef2;
  cursor: pointer;
}

.high-search-content {
  font-size: 13px;
}

.high-search-content ul {
  list-style-type: none;
}

.high-search-content ul {
  margin-left: -14px;
}

.high-search-content li {
  margin-bottom: 20px;
}

.high-search-content li:first-child {
  display: flex;
  flex-direction: row;
}

.key-word {
  display: flex;
  flex-direction: row;
}

.color-gray {
  color: gray;
}

.key-left div,
.key-right div {
  border: 1px solid #d7d9e0;
  border-radius: 6px;
  padding: 5px 10px;
}

.key-word input[type="text"] {
  border: none;
}

.key-word input[type="text"]:focus {
  outline: none;
}

.key-word div {
  margin: -2px 5px 12px 5px;
}

.high-search-content li:nth-child(2) {
  margin-top: -25px;
}

.high-search-content li:nth-child(2) select {
  margin-left: 187px;
}

.high-search-content li:nth-child(3) select {
  margin-left: 225px;
}

#way-choose {
  margin-left: 210px;
}

.high-search-content select {
  border: 1px solid #d7d9e0;
  border-radius: 6px;
  padding: 5px 10px;
}

.web {
  display: inline-block;
  width: 202px;
  height: 26px;
  border: 1px solid #d7d9e0;
  border-radius: 6px;
  margin-left: 172px;
}

.web > input {
  border: none;
  margin: 5px;
}

.web > input:focus {
  outline: none;
}

.high-search-content {
  margin-top: -22px;
}

#high-btn button {
  float: right;
  border: none;
  border-radius: 6px;
  color: #fff;
  background-color: #4e6ef2;
  padding: 5px 12px;
  margin-right: 310px;
  cursor: pointer;
}

#high-btn button:hover {
  background-color: #4662d9;
}

#set-choose,
#way-choose input,
#way-choose label,
.high-search-content select {
  cursor: pointer;
}
</style>
