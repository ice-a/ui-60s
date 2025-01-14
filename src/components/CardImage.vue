<template>
    <a-card hoverable :style="{ width: '360px' }">
      <template #cover>
        <div :style="{
            height: '204px',
            overflow: 'hidden',
        }">
          <img :style="{ width: '100%', transform: 'translateY(-20px)' }" alt="dessert"
               :src="bing.image_url" />
        </div>
      </template>
      <a-card-meta :title="bing.headline">
        <template #description>
          {{ bing.description }}
        </template>
      </a-card-meta>
    </a-card>
  </template>
  
  <script>
  export default {
    props: {
      bing: Object
    }
  };
  </script>
  
  <style scoped>
  /* 可以添加一些自定义样式来进一步美化界面 */
  </style>