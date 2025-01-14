<template>

  <div :style="{ display: 'flex' }">
    <a-card :style="{ width: '360px' }" v-for="item in list" :key="item.title" :title="item.title">
            <span v-if="item.description">{{ item.description }}</span>
    </a-card>
  </div>

  </template>
  
  <script>
  export default {
    props: {
      title: String,
      list: Array
    }
  };
  </script>
  
  <style scoped>
  /* 可以添加一些自定义样式来进一步美化界面 */
  </style>