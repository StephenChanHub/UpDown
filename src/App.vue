<template>
  <div class="app-shell">
    <!-- 内容层 -->
    <main class="content-layer">
      <section class="feed-card" v-for="n in 6" :key="n">
        <h3>Content Block {{ n }}</h3>
        <p>这里是页面内容层，用于演示与悬浮组件的层级分离效果。</p>
      </section>
    </main>

    <!-- 悬浮层 -->
    <div class="floating-layer">
      <header class="floating-top">
        <div class="search-box" :class="{ focused: isSearchFocused }">
          <span class="search-icon">🔍</span>
          <input
            type="text"
            placeholder="Search"
            @focus="isSearchFocused = true"
            @blur="isSearchFocused = false"
          />
        </div>

        <button class="user-btn" type="button">User</button>
      </header>

      <button class="add-btn" type="button">＋</button>

      <nav class="bottom-nav">
        <button
          v-for="item in navItems"
          :key="item.id"
          class="nav-item"
          :class="{ active: activeNav === item.title }"
          type="button"
          @click="activeNav = item.title"
        >
          {{ item.title }}
        </button>
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
  { id: 5, title: 'Me' },
]);
</script>

<style scoped>
.app-shell {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: #f2f2f7;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Text', sans-serif;
}

/* 第一层：页面内容 */
.content-layer {
  position: relative;
  z-index: 1;
  height: 100%;
  overflow-y: auto;
  padding: 110px 20px 140px;
}

.feed-card {
  background: #fff;
  border-radius: 18px;
  padding: 18px;
  margin-bottom: 14px;
  box-shadow: 0 8px 22px rgba(0, 0, 0, 0.05);
}

.feed-card h3 {
  margin: 0 0 6px;
  font-size: 16px;
}

.feed-card p {
  margin: 0;
  color: #6c6c70;
  font-size: 14px;
}

/* 第二层：悬浮组件层 */
.floating-layer {
  position: absolute;
  inset: 0;
  z-index: 10;
  pointer-events: none;
}

.floating-top {
  position: fixed;
  top: 20px;
  left: 20px;
  right: 20px;
  display: flex;
  align-items: center;
  gap: 12px;
  pointer-events: none;
}

.search-box,
.user-btn,
.bottom-nav,
.add-btn {
  pointer-events: auto;
}

.search-box {
  flex: 1;
  max-width: 500px;
  display: flex;
  align-items: center;
  padding: 10px 14px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.72);
  border: 1px solid rgba(255, 255, 255, 0.55);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  box-shadow: 0 10px 28px rgba(0, 0, 0, 0.08);
  transition: all 0.28s ease;
}

.search-box.focused {
  transform: translateY(-1px);
  box-shadow: 0 12px 30px rgba(0, 122, 255, 0.2);
  border-color: rgba(0, 122, 255, 0.25);
}

.search-icon {
  opacity: 0.55;
}

.search-box input {
  margin-left: 8px;
  width: 100%;
  border: none;
  background: transparent;
  outline: none;
  font-size: 14px;
}

.user-btn {
  width: 44px;
  height: 44px;
  border: none;
  border-radius: 50%;
  font-size: 11px;
  font-weight: 600;
  background: rgba(255, 255, 255, 0.75);
  backdrop-filter: blur(16px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
}

.add-btn {
  position: fixed;
  bottom: 96px;
  left: 50%;
  transform: translateX(-50%);
  width: 54px;
  height: 54px;
  border: none;
  border-radius: 50%;
  font-size: 30px;
  line-height: 1;
  color: #fff;
  background: linear-gradient(145deg, #1c8dff, #007aff);
  box-shadow: 0 10px 26px rgba(0, 122, 255, 0.38);
}

.bottom-nav {
  position: fixed;
  left: 50%;
  bottom: 24px;
  transform: translateX(-50%);
  display: flex;
  gap: 6px;
  padding: 9px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.74);
  border: 1px solid rgba(255, 255, 255, 0.56);
  backdrop-filter: blur(18px);
  box-shadow: 0 14px 34px rgba(0, 0, 0, 0.11);
}

.nav-item {
  border: none;
  padding: 8px 12px;
  border-radius: 999px;
  background: transparent;
  color: #7c7c82;
  font-size: 12px;
  font-weight: 600;
}

.nav-item.active {
  color: #fff;
  background: #007aff;
}
</style>
