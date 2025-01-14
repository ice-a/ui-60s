<template>
  <a-table :columns="columns" :data="historyData" ellipsis >
    <template #columns>
      <a-table-column title="事件" data-index="title"></a-table-column>
      <a-table-column title="时间" data-index="year" size="large">
      </a-table-column>
      <a-table-column title="类型" data-index="type"></a-table-column>
      <a-table-column title="简介" data-index="desc"></a-table-column>
      <a-table-column title="link">
        <template #cell="{ record }">
          <a-link :href="record.link">Link</a-link>
        </template>
      </a-table-column>
    </template>
  </a-table>
</template>

<script>
export default {
  props: {
    historyData: Array
  },
  data() {
    return {
      columns: [
        {
          title: '事件',
          dataIndex: 'title',
          width: undefined, // 自适应宽度
        },
        {
          title: '年份',
          dataIndex: 'year',
          width: undefined, // 自适应宽度
        },
        {
          title: '类型',
          dataIndex: 'type',
          width: undefined, // 自适应宽度
        },
        {
          title: 'desc',
          dataIndex: 'desc',
          width: undefined, // 自适应宽度
        },
        {
          title: 'link',
          dataIndex: 'link',
          width: undefined, // 自适应宽度
        },
      ],
    };
  }
};
</script>

<style scoped>
/* 限制表格的最大宽度 */
.arco-table {
  width: 100%;
  max-width: 100%; /* 限制表格的最大宽度 */
  table-layout: auto; /* 让列宽根据内容自适应 */
}

/* 让内容自动换行 */
.arco-table td {
  white-space: normal !important; /* 允许内容换行 */
}
</style>