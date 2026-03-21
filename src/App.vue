<template>
    <div class="app-container">
        <header class="top-header">
            <div class="user-profile-btn">
                <div class="avatar-placeholder">Avatar</div>
            </div>
        </header>

        <nav class="side-nav-glass">
            <div v-for="item in navItems" :key="item.id" class="nav-item" @click="handleNavClick(item.title)">
                <span class="nav-title">{{ item.title }}</span>
                <div class="icon-placeholder">
                    <i :class="item.iconClass"></i>
                </div>
            </div>
        </nav>

        <main class="main-content">
            <router-view />
            <div v-if="!hasRoute" class="content-placeholder">
                <h1>Street Dance Community</h1>
                <p>探索街舞精神，从这里开始。</p>
            </div>
        </main>
    </div>
</template>

<script setup>
import { ref } from 'vue';

// 导航数据
const navItems = ref([
    { id: 1, title: 'Explore', iconClass: 'icon-explore' },
    { id: 2, title: 'Short', iconClass: 'icon-short' },
    { id: 3, title: 'Store', iconClass: 'icon-store' },
    { id: 4, title: 'Message', iconClass: 'icon-message' },
]);

const hasRoute = ref(false); // 仅作演示用

const handleNavClick = (title) => {
    console.log(`Navigating to: ${title}`);
};
</script>

<style scoped>
/* 基础容器 */
.app-container {
    width: 100vw;
    height: 100vh;
    background: linear-gradient(135deg, #1a1a1a 0%, #0d0d0d 100%);
    /* 街舞风格暗色底 */
    color: #ffffff;
    overflow: hidden;
    position: relative;
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}

/* --- 个人信息按钮 --- */
.top-header {
    position: fixed;
    top: 20px;
    right: 20px;
    z-index: 100;
}

.avatar-placeholder {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    backdrop-filter: blur(10px);
    transition: transform 0.3s ease;
    font-size: 10px;
}

.avatar-placeholder:hover {
    transform: scale(1.1);
    border-color: #ff3e3e;
    /* 加上一个动感的强调色 */
}

/* --- 悬浮玻璃导航栏 --- */
.side-nav-glass {
    position: fixed;
    right: 30px;
    top: 50%;
    transform: translateY(-50%);
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding: 25px 15px;
    z-index: 99;

    /* 玻璃拟态核心代码 */
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(15px);
    -webkit-backdrop-filter: blur(15px);
    border-radius: 40px;
    border: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
}

.nav-item {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    /* 标题在左，图标在右 */
    cursor: pointer;
    padding: 10px 15px;
    border-radius: 20px;
    transition: all 0.3s ease;
}

.nav-title {
    margin-right: 15px;
    font-size: 14px;
    font-weight: 500;
    letter-spacing: 1px;
    text-transform: uppercase;
    opacity: 0.7;
}

.icon-placeholder {
    width: 32px;
    height: 32px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 12px;
}

/* 悬停效果 */
.nav-item:hover {
    background: rgba(255, 255, 255, 0.1);
}

.nav-item:hover .nav-title {
    opacity: 1;
    color: #ff3e3e;
}

.nav-item:hover .icon-placeholder {
    background: #ff3e3e;
    box-shadow: 0 0 15px rgba(255, 62, 62, 0.5);
}

/* 主内容区域 */
.main-content {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.content-placeholder {
    text-align: center;
}
</style>