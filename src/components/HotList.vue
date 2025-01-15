<template>
    <div class="card-container">
        <a-card hoverable :style="{ width: '360px', marginBottom: '20px' }" v-for="(item, index) in dataSource"
            :key="index">
            <template #cover>
                <div :style="{
                    height: '204px',
                    overflow: 'hidden',
                }">
                    <img :style="{ width: '100%', transform: 'translateY(-20px)' }" alt="dessert" :src="item.cover" />
                </div>
            </template>
            <a-card-meta :title="item.title">
                <template #description>
                    {{ item.word }} <br />
                    热度值: {{ item.hot_value }}<br />
                    Time: {{ item.active_time }}
                </template>
            </a-card-meta>
        </a-card>
    </div>
</template>

<script>
export default {
    props: {
        dataSource: {
            type: Array,
            required: true,
        },
    },
};
</script>

<style scoped>
.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    /* 卡片之间的间距 */
    justify-content: flex-start;
    /* 左对齐 */
}

.card-container .arco-card {
    flex: 1 1 calc(25% - 20px);
    /* 每行显示 4 个卡片，减去间距 */
    max-width: calc(25% - 20px);
    /* 每行显示 4 个卡片，减去间距 */
    box-sizing: border-box;
    /* 防止 padding 和 border 影响宽度 */
}
</style>