<template>
    <a-list :size="'medium'" :max-height="500" @reach-bottom="list" :bordered="false" :split="false">
      <template #header>
        {{ title }}
      </template>
      <a-list-item v-for="(item, index) in list" :key="index" :style="{ color: 'blue', fontWeight: 'bold', fontSize: '30px'}" >
        {{ item.word || item.query || item.keyword || item}}
        <span v-if="item.num !== undefined">- Num: {{ item.num }} </span>
        <span v-if="item.rank !== undefined">- Rank: {{ item.rank }} </span>
        
      </a-list-item>
    </a-list>
  </template>
  
  <script>
  export default {
    props: {
      title: String,
      list: Array
    }
  };
  </script>