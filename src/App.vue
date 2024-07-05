<template>
  <div class="common-layout">
    <el-container>
      <el-header class="header">
        <div class="left">
          <img class="logo" src="/logo-full.png" />

          <div class="search">
            <el-input placeholder="题目编号" v-model="pid" @keydown.enter="getStatement" />
          </div>

          
        </div>

        <div class="right">
          <div class="config">
            <el-input v-if="!hideCookie" placeholder="cookie" v-model="cookie" @keydown.enter="hideCookie = true" />

            <font-awesome-icon :icon="faGear" size="2x" @click="hideCookie = !hideCookie" />
          </div>
          
        </div>
      </el-header>

      <el-container style="margin-top: 3.5em;">
        <el-aside width="200px" class="aside">
          <div class="nav">
            <div class="title">
              条目导航
            </div>

            <a href="#background" class="nav-item">
              题目描述
            </a>
            <a href="#description" class="nav-item">
              题目背景
            </a>
            <a href="#format-i" class="nav-item">
              输入格式
            </a>
            <a href="#format-o" class="nav-item">
              输出格式
            </a>
            <a href="#examples" class="nav-item">
              样例组
            </a>
            <a href="#hint" class="nav-item">
              说明提示
            </a>
          </div>
        </el-aside>

        <el-container style="margin-left: 200px;">
          <el-main style="flex-grow: 1; ">
            <div class="content">
              <el-card>
                <div class="title-large">题目列表</div>
              </el-card>

              <el-card style="margin-top: 1em">
                <div class="title-large">妙妙工具</div>
              </el-card>

              <el-card style="margin-top: 1em">
                <div class="title-large">题面详情</div>
                <el-form>
                  <el-form-item label="题目名称" class="title-small">
                    <el-input></el-input>
                  </el-form-item>
                  <el-form-item label="题目背景" class="title-small">
                    <div class="bookmark" id="background" />
                    <casket-star v-model="background" :lang="zhCN" height="300px" class="casket" />
                  </el-form-item>
                  <el-form-item label="题目描述" class="title-small">
                    <div class="bookmark" id="description" />
                    <casket-star v-model="description" :lang="zhCN" height="300px" class="casket" />
                  </el-form-item>
                  <el-form-item label="输入格式" class="title-small">
                    <div class="bookmark" id="format-i" />
                    <casket-star v-model="formatI" :lang="zhCN" height="300px" class="casket" />
                  </el-form-item>
                  <el-form-item label="输出格式" class="title-small">
                    <div class="bookmark" id="format-o" />
                    <casket-star v-model="formatO" :lang="zhCN" height="300px" class="casket" />
                  </el-form-item>
                  <el-form-item label="提示说明" class="title-small">
                    <div class="bookmark" id="hint" />
                    <casket-star v-model="hint" :lang="zhCN" height="300px" class="casket" />
                  </el-form-item>
                  <el-form-item label="设置选项" class="title-small">

                  </el-form-item>
                </el-form>

                <el-button type="primary">
                  确认
                </el-button>

                <el-button type="warning" plain>
                  取消
                </el-button>
              </el-card>
            </div>
            
          </el-main>

          <!-- <el-footer>
            Footer
          </el-footer> -->
        </el-container>
      </el-container>
    </el-container>
  </div>
</template>

<script setup lang="ts">
import 'element-plus/dist/index.css';

import 'casket-star/themes/casket/light.css';
import 'casket-star/themes/markdown/light.css';

import zhCN from 'casket-star/lang/zh_CN.json';

import {
  ElContainer,
  ElMain,
  ElAside,
  ElHeader,
  // ElFooter,
  ElCard,
  ElForm,
  ElFormItem,
  ElInput,
  ElButton,
} from 'element-plus';

import { CasketStar } from 'casket-star';

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import {
  faGear
} from '@fortawesome/free-solid-svg-icons';

import { ref } from 'vue';

import axios from 'axios';

const background = ref('');
const formatI = ref('');
const formatO = ref('');
const description = ref('');
const hint = ref('');

const pid = ref('');

const hideCookie = ref(true);
const cookie = ref('');

function getStatement() {

  console.log(cookie.value);

  document.cookie = cookie.value;

  axios.get(
    `https://www.luogu.com.cn/problem/edit/${ pid.value }`, {
      method: 'GET',
      'headers': {
        'X-Luogu-Type': 'content-only',
        'Content-Type': 'application/json'
      },
      withCredentials: true
    }
  ).then((r) => {
    console.log(r)
  });
}

</script>

<style scoped>

.common-layout {
  min-height: 100vh;
  display: flex;

  background-color: var(--el-color-info-light-9);
}

.logo {
  box-sizing: border-box;
  height: 100%;
  padding: 0.2em;
}

.header {
  display: flex;
  justify-content: space-between;

  border-bottom: 1px solid var(--el-color-info-light-7);
  background-color: white;

  padding: 0 2em;
  width: 100%;

  z-index: 2;

  position: fixed;

  > .left {
    display: flex;

    > .search {
      display: flex;
      align-items: center;
      justify-content: center;

      margin-left: 2em;
    }
  }

  > .right {
    display: flex;

    > .config {
      display: flex;
      align-items: center;
      justify-content: center;
      color: var(--el-color-primary);
      
    }
  }
}

.content {
  margin: 0 auto;
  max-width: 1100px;
}

.aside {
  background-color: white;

  position: fixed;
  height: 100%;
}

.nav {
  > .title {
    font-size: large;

    text-align: center;
    padding: 0.8em 0.6em;
    color: var(--el-color-primary);

    border-bottom: 2px solid var(--el-color-primary);
  }
  > .nav-item {
    padding: 0.8em 0.6em;

    display: block;

    transition:
      ease-in-out 0.2s background-color,
      ease-in-out 0.2s color;

    &:hover {
      background-color: var(--el-color-primary-light-9);
      color: var(--el-color-primary);
    }
  }

  a {
    color: inherit;
    text-decoration: none;
  }
}

.title-small {
  font-weight: bold;
}

.title-large {
  margin: 0 0 1em 0;
  font-weight: lighter;
  font-size: larger;
  color: var(--el-color-primary);
}

.bookmark {
  position: absolute;
  top: -75px;
}

.casket {
  flex-grow: 1;
  line-height: normal;
}
 
</style>
