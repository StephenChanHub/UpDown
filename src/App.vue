<template>
    <div class="ios-app-container">
        <!-- 内容层 -->
        <main class="content-layer">
            <section class="ios-content-card">
                <h2>{{ activeNav }}</h2>
                <p>{{ pageDescriptions[activeNav] }}</p>
            </section>
        </main>

        <!-- 悬浮层 -->
        <div class="floating-layer">
            <div class="top-bar">
                <div class="search-wrapper" :class="{ 'is-focused': isSearchFocused }">
                    <div class="search-icon-placeholder">🔍</div>
                    <input type="text" placeholder="搜索舞者、视频或课程..." @focus="isSearchFocused = true"
                        @blur="isSearchFocused = false" />
                </div>

                <div class="user-profile-btn">
                    <div class="avatar-placeholder">User</div>
                </div>
            </div>

            <button class="floating-add-btn" type="button" aria-label="Add">+</button>

            <nav class="side-nav-ios">
                <div v-for="item in navItems" :key="item.id" class="nav-item"
                    :class="{ 'active': activeNav === item.title }" @click="activeNav = item.title">
                    <span class="nav-title">{{ item.title }}</span>
                    <div class="icon-placeholder">
                        <div class="icon-box"></div>
                    </div>
                </div>
            </nav>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const isSearchFocused = ref(false);
const activeNav = ref('Explore');

const navItems = ref([
    { id: 1, title: 'Explore' },
    { id: 2, title: 'Short' },
    { id: 3, title: 'Store' },
    { id: 4, title: 'Message' },
]);

const pageDescriptions = {
    Explore: '探索页内容区域',
    Short: '短视频页内容区域',
    Store: '商店页内容区域',
    Message: '消息页内容区域',
};
</script>

<style scoped>
/* 全局基础：iOS 浅灰色背景 */
.ios-app-container {
    width: 100vw;
    height: 100vh;
    background-color: #F2F2F7;
    color: #000000;
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Helvetica Neue", Arial, sans-serif;
    overflow: hidden;
    position: relative;
}

/* 第一层：内容 */
.content-layer {
    position: relative;
    z-index: 1;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 90px 90px 40px 30px;
    box-sizing: border-box;
}

/* 第二层：悬浮组件 */
.floating-layer {
    position: absolute;
    inset: 0;
    z-index: 10;
    pointer-events: none;
}

/* --- 顶部区域布局 --- */
.top-bar {
    position: fixed;
    top: 20px;
    left: 30px;
    right: 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 100;
    pointer-events: none;
}

/* 搜索框：响应式蓝色效果 */
.search-wrapper {
    pointer-events: auto;
    display: flex;
    align-items: center;
    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    padding: 8px 16px;
    border-radius: 12px;
    width: 300px;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    border: 1px solid rgba(0, 0, 0, 0.05);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.06);
}

.search-wrapper input {
    background: transparent;
    border: none;
    outline: none;
    margin-left: 10px;
    width: 100%;
    font-size: 15px;
    color: #1C1C1E;
}

/* 聚焦时的蓝色响应 */
.search-wrapper.is-focused {
    width: 350px;
    background: rgba(255, 255, 255, 0.9);
    border-color: #007AFF;
    /* iOS Blue */
    box-shadow: 0 0 0 4px rgba(0, 122, 255, 0.1);
}

.search-wrapper.is-focused .search-icon-placeholder {
    color: #007AFF;
}

.user-profile-btn {
    pointer-events: auto;
}

/* 个人头像 */
.avatar-placeholder {
    width: 44px;
    height: 44px;
    border-radius: 50%;
    background: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
    font-size: 12px;
    font-weight: 600;
    color: #8E8E93;
    cursor: pointer;
    transition: transform 0.2s;
}

.avatar-placeholder:active {
    transform: scale(0.92);
}

/* 添加按钮悬浮 */
.floating-add-btn {
    pointer-events: auto;
    position: fixed;
    right: 28px;
    bottom: 24px;
    width: 52px;
    height: 52px;
    border: none;
    border-radius: 50%;
    background: #007AFF;
    color: #ffffff;
    font-size: 30px;
    line-height: 1;
    box-shadow: 0 10px 25px rgba(0, 122, 255, 0.35);
    z-index: 101;
    cursor: pointer;
}

/* --- 右侧 iOS 导航栏 --- */
.side-nav-ios {
    pointer-events: auto;
    position: fixed;
    right: 25px;
    top: 50%;
    transform: translateY(-50%);
    display: flex;
    flex-direction: column;
    gap: 12px;
    padding: 15px 10px;
    z-index: 99;

    /* iOS 玻璃质感 */
    background: rgba(255, 255, 255, 0.6);
    backdrop-filter: blur(25px);
    -webkit-backdrop-filter: blur(25px);
    border-radius: 24px;
    border: 0.5px solid rgba(255, 255, 255, 0.3);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
}

.nav-item {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding: 10px 14px;
    border-radius: 16px;
    cursor: pointer;
    transition: all 0.2s ease;
}

.nav-title {
    margin-right: 12px;
    font-size: 14px;
    font-weight: 500;
    color: #3A3A3C;
    /* 深灰 */
}

.icon-placeholder {
    width: 28px;
    height: 28px;
    background: #E5E5EA;
    /* iOS 浅灰 */
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.icon-box {
    width: 12px;
    height: 12px;
    border: 2px solid #8E8E93;
    border-radius: 3px;
}

/* 交互状态：iOS 蓝色激活 */
.nav-item:hover {
    background: rgba(0, 0, 0, 0.03);
}

.nav-item.active .nav-title {
    color: #007AFF;
}

.nav-item.active .icon-placeholder {
    background: #007AFF;
}

.nav-item.active .icon-box {
    border-color: #ffffff;
}

/* 主内容卡片 */
.ios-content-card {
    background: white;
    width: 80%;
    height: 70%;
    border-radius: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.03);
}

.ios-content-card h2 {
    margin: 0;
}

.ios-content-card p {
    margin-top: 8px;
    color: #8E8E93;
}
</style>
