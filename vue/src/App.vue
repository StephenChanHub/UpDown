<template>
    <div class="ios-app-container">
        <header class="top-floating-layer">
            <div class="island-wrapper left">
                <div class="glass-island avatar-btn">
                    <span>User</span>
                </div>
            </div>

            <div class="island-wrapper center">
                <div class="glass-island search-bar" :class="{ 'is-focused': isSearchFocused }">
                    <span class="search-icon">🔍</span>
                    <input type="text" placeholder="search" @focus="isSearchFocused = true"
                        @blur="isSearchFocused = false" />
                </div>
            </div>

            <div class="island-wrapper right">
                <div class="glass-island add-btn">
                    <span class="add-icon">+</span>
                </div>
            </div>
        </header>

        <main class="main-content">
            <transition name="slide-vertical" mode="out-in">
                <component :is="currentView" :key="activeNav" />
            </transition>
        </main>

        <nav class="bottom-floating-nav-layer">
            <div class="glass-island nav-container">
                <div v-for="item in navItems" :key="item.id" class="nav-item"
                    :class="{ 'active': activeNav === item.title }" @click="activeNav = item.title">
                    <div class="nav-icon-wrapper">
                        <div class="icon-dot"></div>
                    </div>
                    <span class="nav-label">{{ item.title }}</span>
                </div>
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
import Me from './views/Me.vue';

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
    // 确保 Me 页面也被正确映射
    const views = { Explore, Short, Store, Message, Me };
    return views[activeNav.value];
});
</script>

<style scoped>
/* 全局基础：iOS 系统背景色，无边框，无额外 div 阴影 */
.ios-app-container {
    width: 100vw;
    height: 100vh;
    background-color: #F2F2F7;
    color: #000000;
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", sans-serif;
    overflow: hidden;
    position: relative;
    margin: 0;
}

/* --- 核心悬浮容器样式 (Glass Island) --- */
/* 这里直接套用了你之前满意的导航栏容器参数 */
.glass-island {
    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(30px) saturate(150%);
    -webkit-backdrop-filter: blur(30px) saturate(150%);
    border-radius: 35px;
    border: 1px solid rgba(255, 255, 255, 0.5);
    box-shadow: 0 15px 45px rgba(0, 0, 0, 0.08);
    display: flex;
    align-items: center;
    transition: all 0.5s cubic-bezier(0.19, 1, 0.22, 1);
    pointer-events: auto;
    /* 恢复点击 */
}

/* --- 顶部悬浮层布局 --- */
.top-floating-layer {
    position: fixed;
    top: 30px;
    left: 0;
    width: 100%;
    height: 60px;
    z-index: 1000;
    display: flex;
    justify-content: center;
    align-items: center;
    pointer-events: none;
    /* 穿透层级：只有内部的岛屿可点，不产生“灰色 div”挡住内容 */
}

.island-wrapper {
    position: absolute;
    display: flex;
    align-items: center;
}

.island-wrapper.left {
    left: 30px;
}

.island-wrapper.center {
    position: relative;
}

/* 搜索框居中 */
.island-wrapper.right {
    right: 30px;
}

/* 搜索岛屿细节 */
.search-bar {
    min-width: 280px;
    padding: 8px 18px;
}

.search-bar.is-focused {
    min-width: 340px;
    background: rgba(255, 255, 255, 0.9);
    box-shadow: 0 15px 45px rgba(0, 122, 255, 0.12);
}

.search-bar input {
    background: transparent;
    border: none;
    outline: none;
    margin-left: 10px;
    width: 100%;
    font-size: 15px;
}

/* 用户与加号岛屿细节 */
.avatar-btn,
.add-btn {
    width: 46px;
    height: 46px;
    justify-content: center;
    cursor: pointer;
}

.avatar-btn span {
    font-size: 10px;
    font-weight: 600;
}

.add-btn span {
    font-size: 24px;
    font-weight: 300;
    color: #007AFF;
}

/* iOS 蓝色加号 */

.avatar-btn:hover,
.add-btn:hover {
    transform: scale(1.08);
}

/* --- 底部导航层布局 --- */
.bottom-floating-nav-layer {
    position: fixed;
    bottom: 34px;
    left: 0;
    width: 100%;
    display: flex;
    justify-content: center;
    z-index: 1000;
    pointer-events: none;
}

.nav-container {
    min-width: 320px;
    padding: 8px 12px;
}

/* 导航项样式 (保持原样) */
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

/* --- 主内容区 (全屏流动) --- */
.main-content {
    width: 100%;
    height: 100%;
    overflow-y: auto;
    /* 预留上下 padding，防止被岛屿遮挡第一行内容 */
    padding: 10px 20px 20px 20px;
    box-sizing: border-box;
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