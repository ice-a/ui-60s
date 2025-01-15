<template>
  <a-tabs default-active-key="2">
    <a-tab-pane key="1" title="epic游戏">
      <a-col :span="8" :offset="8">
        <Card :title="epicGames.title" :list="epicGames.list" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="2" title="每日新闻60s">
      <a-col :span="8" :offset="8">
        <List :title="'每日新闻60s'" :list="newsList" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="3">
      <template #title>bing壁纸</template>
      <a-col :span="8" :offset="8">
        <CardImage :bing="bing" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="4">
      <template #title>历史上的今天</template>
      <a-col :span="16" :offset="4">
        <HistoryTable :historyData="historyData" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="5">
      <template #title>bili热榜</template>
      <a-col :span="8" :offset="8">
        <List :title="'bili热榜'" :list="biliHotSearch" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="6">
      <template #title>微博热榜</template>
      <a-col :span="8" :offset="8">
        <List :title="'微博热榜'" :list="weiboHotSearch" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="7">
      <template #title>知乎热榜</template>
      <a-col :span="8" :offset="8">
        <List :title="'知乎热榜'" :list="zhihuHotSearch" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="8">
      <template #title>头条热榜</template>
      <a-col :span="8" :offset="8">
        <List :title="'头条热榜'" :list="toutiaoHotSearch" />
      </a-col>
    </a-tab-pane>
    <a-tab-pane key="9">
      <template #title>抖音热榜</template>
      <a-col :span="16" :offset="4">
        <HotTop :dataSource="douyinHotSearch" />
      </a-col>
    </a-tab-pane>
  </a-tabs>
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
      epicGames: {},  // epic信息
      newsList: [],  // 每日新闻60s
      bing: {},  // bing壁纸
      historyData: [],  // 历史上的今天
      biliHotSearch: {},  // bili热榜
      weiboHotSearch: {},  // 微博热榜
      zhihuHotSearch: {},  // 知乎热榜
      toutiaoHotSearch: {},  // 头条热榜
      douyinHotSearch: {},  // 抖音热榜
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
  },
  methods: {
    // epic游戏
    async fetchEpicGames() {
      await axios.get(`${apiBaseUrl}/epic`).then(response => {
        //console.log('log:', response.data);
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
        //console.log('新闻log', response.data);
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
        //console.log('新闻log', response.data);
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
        //console.log('历史log', response.data);
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
        //console.log('新闻log', response.data);
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
        //console.log('新闻log', response.data);
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
        //console.log('新闻log', response.data);
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
        //console.log('新闻log', response.data);
        if (response.data.data) {
          this.zhihuHotSearch = response.data.data.map(item => ({
            ...item,
          }));
        } else {
          console.error('响应数据中没有 news 属性');
        }
      } catch (error) {
        //console.error('请求失败:', error);
        this.zhihuHotSearch = ['无法获取新闻数据，请稍后再试。'];
      }
    },
    // 头条热榜
    async fetchToutiaoHotSearch() {
      try {
        const response = await axios.get(`${apiBaseUrl}/toutiao`);
        //console.log('新闻log', response.data);
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
/* 可以添加一些自定义样式来进一步美化界面 */
</style>
