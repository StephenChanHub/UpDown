<template>
    <div class="ios-app-container">
        <header class="top-floating-bar">
            <div class="search-island" :class="{ 'is-focused': isSearchFocused }">
                <span class="search-icon">🔍</span>
                <input type="text" placeholder="search" @focus="isSearchFocused = true"
                    @blur="isSearchFocused = false" />
            </div>

            <div class="user-island">
                <div class="avatar-placeholder">User</div>
            </div>
        </header>

        <main class="main-content">
            <transition name="slide-vertical" mode="out-in">
                <component :is="currentView" :key="activeNav" />
            </transition>
        </main>

        <nav class="bottom-floating-nav">
            <div v-for="item in navItems" :key="item.id" class="nav-item"
                :class="{ 'active': activeNav === item.title }" @click="activeNav = item.title">
                <div class="nav-icon-wrapper">
                    <div class="icon-dot"></div>
                </div>
                <span class="nav-label">{{ item.title }}</span>
            </div>
        </nav>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import Explore from './views/Explore.vue';
import Short from './views/Short.vue';
import Store from './views/Store.vue';
import Message from './views/Message.vue';

const isSearchFocused = ref(false);
const activeNav = ref('Explore');

const navItems = ref([
    { id: 1, title: 'Explore' },
    { id: 2, title: 'Short' },
    { id: 3, title: 'Store' },
    { id: 4, title: 'Message' },
    { id: 5, title: 'Me' },

]);

const currentView = computed(() => {
    const views = { Explore, Short, Store, Message };
    return views[activeNav.value];
});
</script>

<style scoped>
/* 全局基础 */
.ios-app-container {
    width: 100vw;
    height: 100vh;
    background-color: #F2F2F7;
    color: #000000;
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", sans-serif;
    overflow: hidden;
    position: relative;
}

/* --- 核心优化：搜索框悬浮岛屿 --- */
.top-floating-bar {
    position: fixed;
    top: 30px;
    left: 0;
    width: 100%;
    display: flex;
    justify-content: center;
    /* 搜索框水平居中 */
    align-items: center;
    z-index: 1000;
    pointer-events: none;
    /* 穿透容器，只点击内部岛屿 */
}

.search-island {
    pointer-events: auto;
    display: flex;
    align-items: center;
    width: 280px;
    padding: 10px 18px;
    /* 极致毛玻璃 */
    background: rgba(255, 255, 255, 0.6);
    backdrop-filter: blur(25px) saturate(180%);
    -webkit-backdrop-filter: blur(25px) saturate(180%);
    /* 悬浮关键：圆角 + 细边框 + 弥散阴影 */
    border-radius: 30px;
    border: 1px solid rgba(255, 255, 255, 0.4);
    box-shadow: 0 12px 35px rgba(0, 0, 0, 0.08);
    transition: all 0.5s cubic-bezier(0.19, 1, 0.22, 1);
}

.search-island.is-focused {
    width: 340px;
    background: rgba(255, 255, 255, 0.9);
    box-shadow: 0 15px 45px rgba(0, 122, 255, 0.15);
    border-color: rgba(0, 122, 255, 0.3);
}

.search-island input {
    background: transparent;
    border: none;
    outline: none;
    margin-left: 10px;
    width: 100%;
    font-size: 15px;
}

.search-icon {
    font-size: 16px;
    opacity: 0.6;
}

/* 个人头像岛屿 */
.user-island {
    pointer-events: auto;
    position: absolute;
    right: 30px;
}

.avatar-placeholder {
    width: 44px;
    height: 44px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(15px);
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid rgba(255, 255, 255, 0.5);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.06);
    font-size: 10px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.3s ease;
}

.avatar-placeholder:hover {
    transform: scale(1.08);
    /* 恢复轻微放大 */
}

/* --- 底部导航栏：严格保持原内部样式 --- */
.bottom-floating-nav {
    position: fixed;
    bottom: 34px;
    left: 50%;
    transform: translateX(-50%);
    min-width: 320px;
    display: flex;
    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(30px) saturate(150%);
    padding: 8px 12px;
    border-radius: 35px;
    border: 1px solid rgba(255, 255, 255, 0.5);
    box-shadow: 0 15px 45px rgba(0, 0, 0, 0.08);
    z-index: 1000;
}

.nav-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 8px 16px;
    border-radius: 25px;
    cursor: pointer;
    transition: all 0.3s;
    flex: 1;
}

.nav-label {
    font-size: 10px;
    font-weight: 600;
    color: #8E8E93;
    margin-top: 4px;
}

.nav-icon-wrapper {
    width: 20px;
    height: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.icon-dot {
    width: 8px;
    height: 8px;
    background: #C7C7CC;
    border-radius: 50%;
    transition: all 0.3s;
}

.nav-item.active .icon-dot {
    background: #007AFF;
    transform: scale(1.4);
}

.nav-item.active .nav-label {
    color: #007AFF;
}

/* --- 动画与内容 --- */
.main-content {
    width: 100%;
    height: 100%;
    overflow-y: auto;
    padding-top: 100px;
    /* 为顶部悬浮留出空间 */
    padding-bottom: 120px;
    /* 为底部悬浮留出空间 */
}

.slide-vertical-enter-active,
.slide-vertical-leave-active {
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.slide-vertical-enter-from {
    opacity: 0;
    transform: translateY(30px);
}

.slide-vertical-leave-to {
    opacity: 0;
    transform: translateY(-30px);
}
</style>