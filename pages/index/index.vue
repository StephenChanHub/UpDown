<template>
  <view class="ios-app-container">
    <view class="top-bar">
      <view class="search-wrapper" :class="{ 'is-focused': isSearchFocused }">
        <text class="search-icon-placeholder">🔍</text>
        <input
          class="search-input"
          type="text"
          placeholder="搜索舞者、视频或课程..."
          @focus="isSearchFocused = true"
          @blur="isSearchFocused = false"
        />
      </view>

      <view class="user-profile-btn">
        <view class="avatar-placeholder">User</view>
      </view>
    </view>

    <view class="side-nav-ios">
      <view
        v-for="item in navItems"
        :key="item.id"
        class="nav-item"
        :class="{ active: activeNav === item.title }"
        @tap="activeNav = item.title"
      >
        <text class="nav-title">{{ item.title }}</text>
        <view class="icon-placeholder">
          <view class="icon-box"></view>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue'

const isSearchFocused = ref(false)
const activeNav = ref('Explore')

const navItems = ref([
  { id: 1, title: 'Explore' },
  { id: 2, title: 'Short' },
  { id: 3, title: 'Store' },
  { id: 4, title: 'Message' },
])
</script>

<style scoped>
.ios-app-container {
  width: 100vw;
  height: 100vh;
  background-color: #f2f2f7;
  color: #000;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Text', 'Helvetica Neue', Arial, sans-serif;
  overflow: hidden;
  position: relative;
}

.top-bar {
  position: fixed;
  top: 20px;
  left: 30px;
  right: 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  z-index: 100;
}

.search-wrapper {
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
}

.search-icon-placeholder {
  color: #8e8e93;
}

.search-input {
  background: transparent;
  border: none;
  outline: none;
  margin-left: 10px;
  width: 100%;
  font-size: 15px;
  color: #1c1c1e;
}

.search-wrapper.is-focused {
  width: 350px;
  background: rgba(255, 255, 255, 0.9);
  border-color: #007aff;
  box-shadow: 0 0 0 4px rgba(0, 122, 255, 0.1);
}

.search-wrapper.is-focused .search-icon-placeholder {
  color: #007aff;
}

.avatar-placeholder {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  font-size: 12px;
  font-weight: 600;
  color: #8e8e93;
  transition: transform 0.2s;
}

.avatar-placeholder:active {
  transform: scale(0.92);
}

.side-nav-ios {
  position: fixed;
  right: 25px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 15px 10px;
  z-index: 99;
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
  transition: all 0.2s ease;
}

.nav-title {
  margin-right: 12px;
  font-size: 14px;
  font-weight: 500;
  color: #3a3a3c;
}

.icon-placeholder {
  width: 28px;
  height: 28px;
  background: #e5e5ea;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-box {
  width: 12px;
  height: 12px;
  border: 2px solid #8e8e93;
  border-radius: 3px;
}

.nav-item:hover {
  background: rgba(0, 0, 0, 0.03);
}

.nav-item.active .nav-title {
  color: #007aff;
}

.nav-item.active .icon-placeholder {
  background: #007aff;
}

.nav-item.active .icon-box {
  border-color: #fff;
}
</style>
