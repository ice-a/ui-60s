<template>
  <a-layout style="height: 100vh;">
    <!-- 加载动画 -->
    <div v-if="isBackgroundLoading" class="loading-overlay">
      <a-spin size="large" tip="加载中..." />
    </div>

    <!-- Header -->
    <a-layout-header style="padding-top: 20px; background: transparent;">
      <a-row>
        <a-col :span="24">
          <a-space :size="'small'" wrap>
            <a-button shape="round" @click="handleTabChange('1')">epic游戏</a-button>
            <a-button shape="round" @click="handleTabChange('2')">每日新闻60s</a-button>
            <a-button shape="round" @click="handleTabChange('3')">bing壁纸</a-button>
            <a-button shape="round" @click="handleTabChange('4')">历史上的今天</a-button>
            <a-button shape="round" @click="handleTabChange('5')">bili热榜</a-button>
            <a-button shape="round" @click="handleTabChange('6')">微博热榜</a-button>
            <a-button shape="round" @click="handleTabChange('7')">知乎热榜</a-button>
            <a-button shape="round" @click="handleTabChange('8')">头条热榜</a-button>
            <a-button shape="round" @click="handleTabChange('9')">抖音热榜</a-button>
          </a-space>
        </a-col>
      </a-row>
    </a-layout-header>

    <!-- Content -->
    <a-layout-content style="margin-top: 20px; width: 100%; padding: 0 10px;">
      <div v-if="activeKey === '1'">
        <a-col :span="24">
          <Card :title="epicGames.title" :list="epicGames.list" />
        </a-col>
      </div>
      <div v-if="activeKey === '2'">
        <a-col :span="24">
          <List :title="'每日新闻60s'" :list="newsList" />
        </a-col>
      </div>
      <div v-if="activeKey === '3'">
        <a-col :span="24">
          <CardImage :bing="bing" />
        </a-col>
      </div>
      <div v-if="activeKey === '4'">
        <a-col :span="24">
          <HistoryTable :historyData="historyData" />
        </a-col>
      </div>
      <div v-if="activeKey === '5'">
        <a-col :span="24">
          <List :title="'bili热榜'" :list="biliHotSearch" />
        </a-col>
      </div>
      <div v-if="activeKey === '6'">
        <a-col :span="24">
          <List :title="'微博热榜'" :list="weiboHotSearch" />
        </a-col>
      </div>
      <div v-if="activeKey === '7'">
        <a-col :span="24">
          <List :title="'知乎热榜'" :list="zhihuHotSearch" />
        </a-col>
      </div>
      <div v-if="activeKey === '8'">
        <a-col :span="24">
          <List :title="'头条热榜'" :list="toutiaoHotSearch" />
        </a-col>
      </div>
      <div v-if="activeKey === '9'">
        <a-col :span="24">
          <HotTop :dataSource="douyinHotSearch" />
        </a-col>
      </div>
    </a-layout-content>
  </a-layout>
</template>

<script>
import axios from 'axios';
import Card from './components/Card.vue';
import List from './components/List.vue';
import CardImage from './components/CardImage.vue';
import HistoryTable from './components/Table.vue';
import HotTop from './components/HotList.vue';

const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;

export default {
  components: {
    Card,
    List,
    CardImage,
    HistoryTable,
    HotTop
  },
  data() {
    return {
      activeKey: '2', // 默认激活的 tab
      epicGames: {},  // epic信息
      newsList: [],  // 每日新闻60s
      bing: {},  // bing壁纸
      historyData: [],  // 历史上的今天
      biliHotSearch: {},  // bili热榜
      weiboHotSearch: {},  // 微博热榜
      zhihuHotSearch: {},  // 知乎热榜
      toutiaoHotSearch: {},  // 头条热榜
      douyinHotSearch: {},  // 抖音热榜
      backgroundImage: '', // 当前背景图 URL
      backgroundUrls: [
        'https://t.alcy.cc/mp',
        'https://www.loliapi.com/acg',
        'https://www.dmoe.cc/random.php',
        'https://api.btstu.cn/sjbz/api.php',
        'https://t.alcy.cc/ys',
        'https://img.paulzzh.com/touhou/random',
        'https://t.alcy.cc/moez',
        'https://t.alcy.cc/ycy',
      ], // 从多个平台获取的背景图 URL 列表
      isBackgroundLoading: true, // 背景图加载状态
    };
  },
  async created() {
    await this.fetchEpicGames();
    await this.fetchNews();
    await this.fetchBing();
    await this.fetchHistoryData();
    await this.fetchDouyinHotSearch();
    await this.fetchBiliHotSearch();
    await this.fetchWeiboHotSearch();
    await this.fetchZhihuHotSearch();
    await this.fetchToutiaoHotSearch();
    await this.setRandomBackground(); // 设置随机背景图
  },
  methods: {
    handleTabChange(key) {
      this.activeKey = key;
    },
    // 设置随机背景图
    async setRandomBackground() {
      if (this.backgroundUrls.length > 0) {
        const randomIndex = Math.floor(Math.random() * this.backgroundUrls.length);
        const url = this.backgroundUrls[randomIndex];
        await this.loadBackgroundImage(url);
      } else {
        console.error('没有可用的背景图 URL');
        this.isBackgroundLoading = false; // 停止加载动画
      }
    },
    // 加载背景图
    loadBackgroundImage(url) {
      return new Promise((resolve) => {
        const img = new Image();
        img.src = url;
        img.onload = () => {
          this.backgroundImage = url;
          document.body.style.backgroundImage = `url(${url})`;
          document.body.style.backgroundSize = 'cover';
          document.body.style.backgroundPosition = 'center';
          this.isBackgroundLoading = false; // 停止加载动画
          resolve();
        };
        img.onerror = () => {
          console.error('背景图加载失败:', url);
          this.isBackgroundLoading = false; // 停止加载动画
          resolve();
        };
      });
    },
    // epic游戏
    async fetchEpicGames() {
      await axios.get(`${apiBaseUrl}/epic`).then(response => {
        this.epicGames.list = [];
        if (response.data.data) {
          this.epicGames.list = response.data.data.map(game => ({
            title: game.title,
            description: game.description,
          }));
        } else {
          console.error('响应数据格式不正确');
        }
      }).catch(error => console.log(error));
    },
    // 每日新闻60s
    async fetchNews() {
      try {
        const response = await axios.get(`${apiBaseUrl}/?v2=1`);
        if (response.data.data && response.data.data.news) {
          this.newsList = response.data.data.news;
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        console.error('请求失败:', error);
        this.newsList = ['无法获取新闻数据，请稍后再试。'];
      }
    },
    // bing图片
    async fetchBing() {
      try {
        const response = await axios.get(`${apiBaseUrl}/bing`);
        if (response.data.data) {
          this.bing = response.data.data;
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        console.error('请求失败:', error);
        this.newsList = ['无法获取新闻数据，请稍后再试。'];
      }
    },
    // 历史上的今天
    async fetchHistoryData() {
      try {
        const response = await axios.get(`${apiBaseUrl}/today_in_history`);
        if (response.data.data) {
          this.historyData = response.data.data.map(item => ({
            ...item,
          }));
        } else {
          console.error('响应数据中没有 data 属性');
          this.historyData = [];
        }
      } catch (error) {
        console.error('请求失败:', error);
        this.historyData = [];
      }
    },
    // 抖音热榜
    async fetchDouyinHotSearch() {
      try {
        const response = await axios.get(`${apiBaseUrl}/douyin`);
        if (response.data.data) {
          this.douyinHotSearch = response.data.data.map(item => ({
            ...item,
          }));
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        console.error('请求失败:', error);
        this.douyinHotSearch = ['无法获取新闻数据，请稍后再试。'];
      }
    },
    // bili热榜
    async fetchBiliHotSearch() {
      try {
        const response = await axios.get(`${apiBaseUrl}/bili`);
        if (response.data.data) {
          this.biliHotSearch = response.data.data.map(item => ({
            ...item,
          }));
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        console.error('请求失败:', error);
        this.biliHotSearch = ['无法获取新闻数据，请稍后再试。'];
      }
    },
    // 微博热榜
    async fetchWeiboHotSearch() {
      try {
        const response = await axios.get(`${apiBaseUrl}/weibo`);
        if (response.data.data) {
          this.weiboHotSearch = response.data.data.map(item => ({
            ...item,
          }));
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        console.error('请求失败:', error);
        this.weiboHotSearch = ['无法获取新闻数据，请稍后再试。'];
      }
    },
    // 知乎热榜
    async fetchZhihuHotSearch() {
      try {
        const response = await axios.get(`${apiBaseUrl}/zhihu`);
        if (response.data.data) {
          this.zhihuHotSearch = response.data.data.map(item => ({
            ...item,
          }));
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        this.zhihuHotSearch = ['无法获取新闻数据，请稍后再试。'];
      }
    },
    // 头条热榜
    async fetchToutiaoHotSearch() {
      try {
        const response = await axios.get(`${apiBaseUrl}/toutiao`);
        if (response.data.data) {
          this.toutiaoHotSearch = response.data.data.map(item => ({
            ...item,
          }));
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        console.error('请求失败:', error);
        this.toutiaoHotSearch = ['无法获取新闻数据，请稍后再试。'];
      }
    },
  }
};
</script>

<style>
/* 全局样式 */
body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  background-color: #f0f2f5;
  /* 默认背景色 */
  transition: background-image 0.5s ease-in-out;
  /* 背景图切换动画 */
  overflow: hidden; /* 禁止页面滚动 */
}

/* 加载动画样式 */
.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.8);
  /* 半透明背景 */
  z-index: 1000;
  /* 确保在最上层 */
}

/* 背景图样式 */
body::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  background-image: var(--background-image);
  /* 动态背景图 */
  background-size: cover;
  background-position: center;
}

/* 媒体查询适配手机屏幕 */
@media (max-width: 768px) {
  .ant-layout-header {
    padding: 10px !important;
  }

  .ant-space {
    justify-content: center;
  }

  .ant-btn {
    margin: 5px;
  }

  .ant-layout-content {
    padding: 0 10px !important;
  }
}
</style>