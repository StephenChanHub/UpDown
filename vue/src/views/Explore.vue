<template>
    <div class="explore-scroll-wrapper">
        <header class="view-header">
        </header>

        <div class="waterfall-container">
            <div class="waterfall-column">
                <PostCard v-for="post in leftColumnPosts" :key="post.id" class="waterfall-item" />
            </div>

            <div class="waterfall-column">
                <PostCard v-for="post in rightColumnPosts" :key="post.id" class="waterfall-item" />
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import PostCard from '../components/PostCard.vue';

// 模拟数据：在实际项目中，这些数据将来自 API
const posts = ref([
    { id: 1 }, { id: 2 }, { id: 3 }, { id: 4 },
    { id: 5 }, { id: 6 }, { id: 7 }, { id: 8 }
]);

/**
 * 瀑布流逻辑：将帖子简单地分配到左右两列
 * 进阶建议：可以根据卡片高度动态分配，确保两列高度接近
 */
const leftColumnPosts = computed(() => posts.value.filter((_, i) => i % 2 === 0));
const rightColumnPosts = computed(() => posts.value.filter((_, i) => i % 2 !== 0));
</script>

<style scoped>
.explore-scroll-wrapper {
    width: 100%;
    height: 100%;
    /* 允许内部滚动 */
    overflow-y: auto;
    padding: 20px;
    box-sizing: border-box;
    /* 隐藏滚动条 (可选，保持 iOS 洁净感) */
    -ms-overflow-style: none;
    scrollbar-width: none;
}

.explore-scroll-wrapper::-webkit-scrollbar {
    display: none;
}

/* --- 标题样式 --- */
.view-header {
    margin-bottom: 24px;
    padding-left: 10px;
}

.view-title {
    font-size: 34px;
    /* iOS Large Title 规格 */
    font-weight: 700;
    color: #1C1C1E;
    margin: 0;
}

.view-subtitle {
    font-size: 15px;
    color: #8E8E93;
    margin: 4px 0 0 0;
}

/* --- 瀑布流布局核心 --- */
.waterfall-container {
    display: flex;
    gap: 20px;
    /* 列间距 */
    align-items: flex-start;
    width: 100%;
    max-width: 1000px;
    margin: 0 auto;
}

.waterfall-column {
    flex: 1;
    /* 左右列等宽 */
    display: flex;
    flex-direction: column;
    gap: 16px;
    /* 卡片垂直间距 */
}

/* 适配 PostCard 在瀑布流中的样式 */
.waterfall-item {
    width: 100% !important;
    margin: 0 !important;
    /* 覆盖原有的 margin */
}

/* 移动端适配 (针对你未来的 uni-app 落地) */
@media (max-width: 600px) {
    .waterfall-container {
        gap: 10px;
    }

    .waterfall-column {
        gap: 10px;
    }
}
</style>