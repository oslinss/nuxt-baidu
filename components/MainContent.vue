<template>
  <div class="main">
    <img
      src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
      alt="baidu"
    />
    <form class="search" id="searchForm" @submit.prevent="performSearch">
      <div class="text-div">
        <input
          type="text"
          v-model="searchQuery"
          id="searchInput"
          ref="searchInput"
          @focus="handleFocus"
          placeholder="韩方发感谢信致谢中方救援"
        />
        <div class="image" @click="focusSearchInput">
          <div class="image-search">按图片搜索</div>
        </div>
      </div>
      <input type="submit" value="百度一下" />
    </form>
    <div
      v-if="showHistory"
      class="search-history"
      id="searchHistory"
    >
      <div
        v-for="(item, index) in searchHistory"
        :key="index"
        class="history-item"
        @click="fillSearchInput(item)"
      >
        <span>{{ item }}</span>
      </div>
      <div class="close-search">
        <NuxtLink to="/searchset">
        <button @click="openSet">关闭历史</button>
        </NuxtLink>
      </div>
      <SearchSet
        v-if="isSettingVisible"
        @close-set="closeSetting"
        class="close-search-plat"
      />
    </div>
    <div class="change">
      <a
        href="https://top.baidu.com/board?platform=pc&sa=pcindex_entry"
        target="_blank"
        ><span id="original">百度热搜</span><span class="gray"> &gt;</span></a
      >
      <a href="javascript:void(0):" @click="refreshHotSearch"
        ><span class="gray">&#10227;换一换</span></a
      >
    </div>
    <div class="news-list">
      <div v-for="news in newsArray" :key="news.rank" :class="'news-item'">
        <span
          :class="[
            'rank',
            {
              top: news.rank === '↑',
              first: news.rank === '1',
              second: news.rank === '2',
              third: news.rank === '3',
            },
          ]"
        >
          {{ news.rank }}
        </span>
        <a :href="news.link" 
        target="_blank" 
        @click="markAsVisited(news.rank)"
        :class="{ visited: visitedNews.includes(news.rank) }">
          {{ news.title }}
        </a>
        <span v-if="news.hot" class="hot-tag">热</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
const newsArray1 = ref([
  {
    rank: "↑",
    title: "“中国必须有自己特色的大国外交”",
    link: "https://www.baidu.com/s?wd=%E2%80%9C%E4%B8%AD%E5%9B%BD%E5%BF%85%E9%A1%BB%E6%9C%89%E8%87%AA%E5%B7%B1%E7%89%B9%E8%89%B2%E7%9A%84%E5%A4%A7%E5%9B%BD%E5%A4%96%E4%BA%A4%E2%80%9D&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "5",
    title: "于东来建议外地朋友不要来胖东来",
    link: "https://www.baidu.com/s?wd=%E4%BA%8E%E4%B8%9C%E6%9D%A5%E5%BB%BA%E8%AE%AE%E5%A4%96%E5%9C%B0%E6%9C%8B%E5%8F%8B%E4%B8%8D%E8%A6%81%E6%9D%A5%E8%83%96%E4%B8%9C%E6%9D%A5&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "1",
    title: "第一批县城加盟商开始闭店了",
    link: "https://www.baidu.com/s?wd=%E7%AC%AC%E4%B8%80%E6%89%B9%E5%8E%BF%E5%9F%8E%E5%8A%A0%E7%9B%9F%E5%95%86%E5%BC%80%E5%A7%8B%E9%97%AD%E5%BA%97%E4%BA%86&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
    hot: true,
  },
  {
    rank: "6",
    title: "王小洪会见国际刑警组织主席",
    link: "https://www.baidu.com/s?wd=%E7%8E%8B%E5%B0%8F%E6%B4%AA%E4%BC%9A%E8%A7%81%E5%9B%BD%E9%99%85%E5%88%91%E8%AD%A6%E7%BB%84%E7%BB%87%E4%B8%BB%E5%B8%AD&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "2",
    title: "立陶宛要求中国外交官离境 中方回应",
    link: "https://www.baidu.com/s?wd=%E7%AB%8B%E9%99%B6%E5%AE%9B%E8%A6%81%E6%B1%82%E4%B8%AD%E5%9B%BD%E5%A4%96%E4%BA%A4%E5%AE%98%E7%A6%BB%E5%A2%83+%E4%B8%AD%E6%96%B9%E5%9B%9E%E5%BA%94&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "7",
    title: "6岁女孩跑完全马 组委会:处罚其父",
    link: "https://www.baidu.com/s?wd=6%E5%B2%81%E5%A5%B3%E5%AD%A9%E8%B7%91%E5%AE%8C%E5%85%A8%E9%A9%AC+%E7%BB%84%E5%A7%94%E4%BC%9A%EF%BC%9A%E5%A4%84%E7%BD%9A%E5%85%B6%E7%88%B6&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "3",
    title: "让冬季旅游“燃”起来",
    link: "https://www.baidu.com/s?wd=%E8%AE%A9%E5%86%AC%E5%AD%A3%E6%97%85%E6%B8%B8%E2%80%9C%E7%87%83%E2%80%9D%E8%B5%B7%E6%9D%A5&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "8",
    title: "进入韩国国会主楼的军人全部撤出",
    link: "https://www.baidu.com/s?wd=%E8%BF%9B%E5%85%A5%E9%9F%A9%E5%9B%BD%E5%9B%BD%E4%BC%9A%E4%B8%BB%E6%A5%BC%E7%9A%84%E5%86%9B%E4%BA%BA%E5%85%A8%E9%83%A8%E6%92%A4%E5%87%BA&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "4",
    title: "韩国总统尹锡悦宣布解除紧急戒严令",
    link: "https://www.baidu.com/s?wd=%E9%9F%A9%E5%9B%BD%E6%80%BB%E7%BB%9F%E5%B0%B9%E9%94%A1%E6%82%A6%E5%AE%A3%E5%B8%83%E8%A7%A3%E9%99%A4%E7%B4%A7%E6%80%A5%E6%88%92%E4%B8%A5%E4%BB%A4&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "9",
    title: "董浩叔叔 最不容易的就是8090后",
    link: "https://www.baidu.com/s?wd=%E8%91%A3%E6%B5%A9%E5%8F%94%E5%8F%94+%E6%9C%80%E4%B8%8D%E5%AE%B9%E6%98%93%E7%9A%84%E5%B0%B1%E6%98%AF8090%E5%90%8E&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
]);

const newsArray2 = ref([
  {
    rank: "10",
    title: "大国外交的新春之约",
    link: "https://www.baidu.com/s?wd=%E5%A4%A7%E5%9B%BD%E5%A4%96%E4%BA%A4%E7%9A%84%E6%96%B0%E6%98%A5%E4%B9%8B%E7%BA%A6&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "15",
    title: "文心一言全面免费",
    link: "https://www.baidu.com/s?wd=%E6%96%87%E5%BF%83%E4%B8%80%E8%A8%80%E5%85%A8%E9%9D%A2%E5%85%8D%E8%B4%B9&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
    hot: true,
  },
  {
    rank: "11",
    title: "《哪吒2》票房破98亿",
    link: "https://www.baidu.com/s?wd=%E3%80%8A%E5%93%AA%E5%90%922%E3%80%8B%E7%A5%A8%E6%88%BF%E7%A0%B498%E4%BA%BF&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
    hot: true,
  },
  {
    rank: "16",
    title: "外国男子看哪吒母亲化丹一幕哭泣",
    link: "https://www.baidu.com/s?wd=%E5%A4%96%E5%9B%BD%E7%94%B7%E5%AD%90%E7%9C%8B%E5%93%AA%E5%90%92%E6%AF%8D%E4%BA%B2%E5%8C%96%E4%B8%B9%E4%B8%80%E5%B9%95%E5%93%AD%E6%B3%A3&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "12",
    title: "中国女留学生在美遇害 全身多处受伤",
    link: "https://www.baidu.com/s?wd=%E4%B8%AD%E5%9B%BD%E5%A5%B3%E7%95%99%E5%AD%A6%E7%94%9F%E5%9C%A8%E7%BE%8E%E9%81%87%E5%AE%B3+%E5%85%A8%E8%BA%AB%E5%A4%9A%E5%A4%84%E5%8F%97%E4%BC%A4&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "17",
    title: "特朗普和普京通话:希望结束俄乌冲突",
    link: "https://www.baidu.com/s?wd=%E7%89%B9%E6%9C%97%E6%99%AE%E5%92%8C%E6%99%AE%E4%BA%AC%E9%80%9A%E8%AF%9D%EF%BC%9A%E5%B8%8C%E6%9C%9B%E7%BB%93%E6%9D%9F%E4%BF%84%E4%B9%8C%E5%86%B2%E7%AA%81&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "13",
    title: "从百分之五增长看定力与信心",
    link: "https://www.baidu.com/s?wd=%E4%BB%8E%E7%99%BE%E5%88%86%E4%B9%8B%E4%BA%94%E5%A2%9E%E9%95%BF%E7%9C%8B%E5%AE%9A%E5%8A%9B%E4%B8%8E%E4%BF%A1%E5%BF%83&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "18",
    title: "港股科技股大涨 百度涨超10%",
    link: "https://www.baidu.com/s?wd=%E6%B8%AF%E8%82%A1%E7%A7%91%E6%8A%80%E8%82%A1%E5%A4%A7%E6%B6%A8+%E7%99%BE%E5%BA%A6%E6%B6%A8%E8%B6%8510%25&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "14",
    title: "饺子导演画着呢画着呢",
    link: "https://www.baidu.com/s?wd=%E9%A5%BA%E5%AD%90%E5%AF%BC%E6%BC%94%E7%94%BB%E7%9D%80%E5%91%A2%E7%94%BB%E7%9D%80%E5%91%A2&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
  {
    rank: "19",
    title: "特朗普:乌克兰加入北约“不切实际”",
    link: "https://www.baidu.com/s?wd=%E7%89%B9%E6%9C%97%E6%99%AE%EF%BC%9A%E4%B9%8C%E5%85%8B%E5%85%B0%E5%8A%A0%E5%85%A5%E5%8C%97%E7%BA%A6%E2%80%9C%E4%B8%8D%E5%88%87%E5%AE%9E%E9%99%85%E2%80%9D&sa=fyb_n_homepage&rsv_dl=fyb_n_homepage&from=super&cl=3&tn=baidutop10&fr=top1000&rsv_idx=2&hisfilter=1",
  },
]);

const newsArray = ref(newsArray1.value);
const searchQuery = ref("");
const searchHistory = ref([]);
const visitedNews = ref([]);
const showHistory = ref(false);
const isSettingVisible = ref(false);


const focusSearchInput = () => {
  const searchInput = document.getElementById("searchInput");
  if (searchInput) {
    searchInput.focus();
  }
};

const handleFocus = () => {
  showHistory.value = true;
};

const fillSearchInput = (item) => {
  searchQuery.value = item;
  showHistory.value = false;
};

const performSearch = () => {
  if (searchQuery.value) {
    // 将搜索词加入历史记录
    searchHistory.value.push(searchQuery.value);
    
    // 构造百度搜索URL并进行跳转
    const encodedQuery = encodeURIComponent(searchQuery.value);
    window.open(`https://www.baidu.com/s?wd=${encodedQuery}`, "_blank");
    
    // 清空搜索框并隐藏历史记录
    searchQuery.value = "";
    showHistory.value = false;
  }
};

const currentNewsArray = ref(1)

const refreshHotSearch = () => {
  currentNewsArray.value = currentNewsArray.value === 1 ? 2 : 1
  newsArray.value = currentNewsArray.value === 1 
    ? [...newsArray1.value]
    : [...newsArray2.value]
}

const markAsVisited = (rank) => {
  if (!visitedNews.value.includes(rank)) {
    visitedNews.value.push(rank);
  }
};

const openSet = () => {
  isSettingVisible.value = true;
};

const closeSetting = () => {
  isSettingVisible.value = false;
};

const handleClickOutside = (event) => {
      const searchHistory = document.getElementById("searchHistory");
      const searchInput = document.getElementById("searchInput");

      // 如果点击的不是历史记录区域或输入框，则关闭历史记录
      if (
        searchHistory &&
        !searchHistory.contains(event.target) &&
        !searchInput?.contains(event.target)
      ) {
        showHistory.value = false;
      }
};

onMounted(() => {
  document.addEventListener("click", handleClickOutside); 
});

onBeforeUnmount(() => {
  document.removeEventListener("click", handleClickOutside);
});

</script>

<style scoped>
.main {
  width: 650px;
  height: auto;
  margin: 20px auto 0;
  z-index: 0;
}

.main a {
  float: left;
  margin-bottom: 5px;
  color: inherit;
  font: 15px Arial, sans-serif;
  text-decoration: none;
}

img {
  display: block;
  margin: 0 auto;
  width: 270px;
  height: 129px;
}

.search {
  display: flex;
  align-items: stretch;
  margin: 20px 0 45px 0;
  border-radius: 10px;
  z-index: 0;
}

.search .text-div {
  display: flex;
  justify-content: space-between;
  width: 550px;
  height: 40px;
  border: 2px solid #bbb;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
}

.search input[type="submit"] {
  display: block;
  float: left;
  position: relative;
  margin-left: -15px;
  padding: 11px 22px;
  font-size: 17px;
  color: #fff;
  border: none;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
  background-color: #4e6ef2;
  cursor: pointer;
}

.search input[type="text"] {
  width: 450px;
  height: 36px;
  margin-left: 10px;
  border: none;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
  transition: border-color 0.3s;
  outline: none;
  background-color: inherit;
}

.search input[type="submit"]:hover {
  background-color: #4662d9;
}

.search input[type="text"]:focus {
  border-color: #4e6ef2;
  outline: none;
}

.search .text-div:hover {
  border: 2px solid rgb(156, 156, 156);
}

.search .text-div:focus-within {
  border: 2px solid #4e6ef2;
}

.search:focus-within .text-div {
  border-color: #4e6ef2;
  border-bottom-right-radius: 5px;
}

.search .image {
  display: inline-block;
  cursor: pointer;
  width: 100px;
  height: 42px;
  background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAAAXNSR0IArs4c6QAABWBJREFUaAXtWE2MFEUUrlczs0vk4E8iCniBEBON7PKzG2/qhsQoHDSaDMpAuLjADu6ScFtDPHok2SE7w7peVnYQ5sDBAyEkZkm4GPefjYaAuheQH40SE8T56Srfq+5qert7pru3Z9mDM8mkq159773vVb9+9cNY69eagdYMtGZgNWcAmuG8UChtrIrqLsnE5iB7nHMpBVtM8dR3fX3p20H4oPHYAQydLr4HQpyTkq0JcrZ0HB5xkB/1Zw98u1QerRcrgKF8cSeT8nvGZDKaW42GWiIpX//08IEZLYn6XHYAIyNTqXLt+rRkcis5BQYL2L4QigCwD5lkr2m9F59v60qn05VQui7QMmeOsbJx/YQmjzYfJgHez2b3/+qy79vN54tfV5m4hoNrycad36snsP25LzhAyAPGfYdzp891YtoM2oMAg9lsJhR50lFY1LH10Vau8M32x/3wrYYpNDExkfzxxq0Ow+BdTIoN2iwqZSRjW6gPAFf7+zJv4hNF4X9SSsjli1dwIt4gLQD2CxaCcdsC8N+wPd3xyob5np6emi13NeoGkM+f7ahKMYYOtrl0HF141J6AziNHMjcdwtDN4a9KW4xKeR6JP1VfCeZSwA9ms/so5Tw/3xQi8jVp/NCIPH60f+O87VkueWJy9JP0zzjze0xbHm6WQG4jLsTJD+F5A5Q21366Pekg/wBLTAmrxh0yoBYiEDdSci0uRB/c9zMaVVYoXFhXhYe7QPKXhRAmJ2Dr0WcabT1j2oO5jlc3drvTyROAWdvFlEXiAW9r39Hfm16MQmp09OwL/1bEW5Kzl0gPBLu1po1f6e3ddy+KnVOjpU2iUqY1wgwCeNexbGbaacOvjO60ATjzUcirSiJqX/xTMd5GG5wJ0xJ93SgTueEzlxMpPnj0UGbO9tGgQb6H8mfo7R8iGIDowseSALzfgKPa6LRp4MMeyhWKA0wak0j2HRR67aKMxoyanFJYWzOgYaUuoXAPtd6N9nPkxgT2iZAUYghLY4LAWFIN/DBn8Nv5kv7UJhmNEYawkYIgxTo/vxSqA/UXq7SRxkk9imQXGCQODmQ/ntUyehIOpDFmr95SnkTZ1YG+pTinTph2/DeAOW/PPJHf9Fy3HyklwzEVIDJTOqgbhmQjTKwAqNpgXtMHq9KGcZz53bvL9RyqMcTY6YS6ZKMePow8VgBUKtGJaUOyeb+Zd5NQGMRacm7ZcMNC92MFoOs8eZMg9doR6NyJddoIVPQBxArAx94TF8UKgFZYzRgk0CIT6ufEOm2EUnaBYgVA2wO0Z663wDpVSXU5cHcVBrGWXFg23LDQ/VgB0N4GN1OXyZtZFo2x3MWL7fW8qzGBa4Fe8FA36v7IbTtWAMoYT372uCzi+Xjxz0m/N6FkOKYXMtKhfZGbUNR+7JWYyiJuC47jKxgi50QQV9zJ3PD4vK42Kuel0YljaquhSAIcD7upaxRU/DeA1gf6Mjng/Jj9Jmi/w+QOtYvEnSS17bShfRJiSacRsbBjTQmAnClCkMCtAruEXWsjvYQGnVQu4T4JtxrNIU/WY6eQk6K1Er/brAON03a9dlMD0E6synJe91fy6U0h8zrD9Enn0tX+OTk4uVm8PAEkEvZ5mK4L9+It2ubVioF8Ewft38lNyzwptO7Z9vm7f5Rn8a5mO1aOp2vAZvBcej7K8VIbj/XEma8xuZc4kB28fpklbm6bnlsJApwaGd8qa5LOt3VXVbehlewjyTIkobv/8P4Ftx9PChGAgKRAUbsVnnSfONQjT1x834AmWSqVEvf/Kne670b1+Io+8YOlnKe0wat3dSGwov5axlsz0JqB1gz8P2fgP16uH37koJrvAAAAAElFTkSuQmCC);
  background-repeat: no-repeat;
  background-position: 50px;
  background-size: 25px;
}

.search .image:hover {
  background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAAAXNSR0IArs4c6QAABU1JREFUaAXtWE9oHFUY/76ZWf9C6slS9ZJUPNlGdzcpetCAEAwIFsRGELqxLcWLHnqLiOTkMVC9iFTNBg82BUERKoFioKC2ySaNvalNEdRST21FSrIz7/P73uybzM6f7Ewmaw7ug903732/7+/73vfeDECv9SLQi0AvArsZAdwJ5c8cpUeb6L2gkAY6ylMWWUjXS2Rf+GEW/+iI7wAo7ED1DfdlpeALIrqvg642MgLeJYTXVurO122EnINCDpRrVAHyfiQgJ6deDWcnXLTtQ43PcHk7/MKzbQcqJ6lE626DCA5oQQhXWdqXmQxR8AoBPGn49vc71XNTuJGJNwLaVuREhlp334WW8RyHf2zbPrz4Ka5F5CcOh47RrOt6PwHQgxKAX9dYFsB7ieAOk1YHeiJ5+BgN8vJPBkQLJ7MaLzwayzyGX2RVj9PTZpyn3zKFRqbI+fs3OEhKVQHVI4Fgha9z3j8uY0S42JhxnkdEzorsjTc9VibcBV6B54SL+a8B0ueBBLL+BLAaff2wujCFbjAfeUh1oHyUDhK6dU6TpyI8wVAqScmyBy/N4C/BZI6H8nEOguutsjMPpLIhXEFyasuzyCkXb4kO+MZ7l4Ho3jiLP8PG30Ggw43Z0ndpmCzz1YnmiFL4Fa9oXyoecR3JHk5yIuaApM3t6+5iEHnEWwya4+W9oRX4B9HP99xvX/j+I/wrVWkOwrNv0sMbd/kgJHwCLOXbRLiPc/IIB/EhLYpXYk+/MxRNp5gDUtuJ3CWfCW+VSnb58hm8nsMeOHSC9jY9bwQVPiZ8ZNHvJdteuHQGb+aRM3yC+ptNb9k4gehUl+vYCMtIKKOqYgAS+TzGSyVRrvf+RtMbZaUWp4UvygPYUJ4q19x5G+3JxRm8YnRs1Ytu5pljKSc1jriYALQ5EC+j4Wpj0mYrLS1apea9rZreIm/IF8X4GIs4xDRPeUuCjdHTJsI2WGpfFBZXFEVkGGvjSZ3miNsC5w3ucXld5tL4sf/jZ54TmmAUY3M5IYwpLSGFUpAp0zptmt60IbPhVy3Hri19gitmTno/vdy6uXrwakzz3MUoLsyT5bnwCkjOB5Fn4/f2OUNJRsmc0MRBMUyvBPNmMXIrTCEHpNrwBhsVBZIiluPUzn+I62kKhSaYzXSCUZGRhs8yX8gBKZXBhkVaTYp81AiNYaye542tZURBOcaFHDB13teH/tmRSfkmtl1GJuY2UCEH2iTt0qCQA3LCbtpNcshkbJvYdhkZ2UOwQg7I9YDvwUrLIxyUUhmSnfioMYzVRObVMhKR2SYLOSB3G75uzIsqvyy69bG30m+wQlMunwXBgQfzee9HUbcKOSDC+NB6JyiL/Hp48467mLQSMic0c5AJj9yLogblHRc+iaUs8rXgFJ+sp0W5GCh3onKtyaXSVBuq8twgnxn6qiE4vmKcynqpE3xaK+yACG7U7Q/YCTaepiU9dIoQlNkd/pnG5nOTyIvxwmMoRfrCKWSUi0FWyearAn4bbGxDlJ43rNAEs1PGi9gdWQERJK11Eo/t1AuNL3Xr/x11wKhqVZazZtzNPp5C+nNGSyW/l3ZTeSbZYRvCtrWYYw6gZYXvNOP8FW0gk6IugFq6x43oiG16OvZS/+oc2de+4XoNoE9V3ni3GXk2+CphpHW79yM/zpVtj6hiQ1f2v+QMnTviv9kZ9TEHhFCZoAOKPP60kn6qGgH/Sc/fhSzk6jWD+mUorDOWQkIUoDCI12HwbjyLDWnGiz2JK2AMlXRaOw+DsW+jBtDNnjes5PzAGKxG06abanuyexHoRaAXgf9ZBP4FE+w3/NJwpdgAAAAASUVORK5CYII=);
  cursor: pointer;
}

.search .image-search {
  display: none;
  position: absolute;
  width: auto;
  height: auto;
  padding: 5px;
  background-color: #fff;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.15);
  border-radius: 5px;
  z-index: 1000;
  top: 280px;
  font-size: 14px;
  color: grey;
  white-space: nowrap;
}

.search .image:hover .image-search {
  display: block;
}

.search-history {
  position: absolute;
  width: 537px;
  background: #fff;
  border: 2px solid #4e6ef2;
  border-top: none;
  border-radius: 0 0 10px 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-top: -55px;
  z-index: 1001;
}

.search-history span {
  color: #9195a3;
}

.close-search-plat {
  z-index: 1002;
}

.close-search button {
  color: #9195a3;
  background-color: inherit;
  border: none;
  cursor: pointer;
  margin-left: 465px;
  padding: 2px;
}

.close-search:hover {
  color: #4e6ef2;
}

.history-items {
  max-height: 300px;
  overflow-y: auto;
}

.history-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 15px;
  cursor: pointer;
}

.history-item:hover {
  background-color: #f8f9fa;
}

.delete-history {
  cursor: pointer;
  color: #999;
  padding: 0 5px;
}

.delete-history:hover {
  color: #ff4444;
}

.history-text {
  color: #858585;
}

.history-item:hover .history-text {
  color: #4e6ef2;
}

.change {
  display: flex;
  justify-content: space-between;
}

.chang a {
  margin-right:100px;
}

#original {
  font-weight: 600;
  color: inherit;
}

.gray {
  color: gray;
}

.change a:hover,
.gray:hover {
  color: #4e6ef2;
  cursor: pointer;
}

.change > a b:hover {
  color: #4e6ef2;
  cursor: pointer;
}

.news-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 5px;
  margin-top: 20px;
}

.news-item {
  display: flex;
}

.news-list a {
  color: inherit;
  text-decoration: none;
}

.rank {
  float: left;
  padding-right: 10px;
  color: #9195a3;
}

.rank:hover {
  cursor: pointer;
}

.rank.top,
.rank.first {
  color: #fe2d46;
}

.rank.second {
  color: #f60;
}

.rank.third {
  color: #faa90e;
}

.hot-tag {
  height: 17px;
  color: #fff;
  font-size: 12px;
  background-color: #f60;
  border-radius: 3px;
  margin-top: 2px;
  padding: 0 2px;
  margin-left: 5px;
}

.visited {
  color: #626675 !important; /* 浏览过的新闻标题颜色为灰色 */
}

.news-list a:hover,
.rank:hover ~ a {
  color: #4e6ef2 !important;
  text-decoration: underline;
  cursor: pointer;
}
</style>
