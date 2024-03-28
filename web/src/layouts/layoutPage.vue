<!--
* Licensed to the Apache Software Foundation (ASF) under one
* or more contributor license agreements.  See the NOTICE file
* distributed with this work for additional information
* regarding copyright ownership.  The ASF licenses this file
* to you under the Apache License, Version 2.0 (the
* "License"); you may not use this file except in compliance
* with the License.  You may obtain a copy of the License at
*
*     http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
* See the License for the specific language governing permissions and
* limitations under the License.
-->

<script setup lang="ts">
import { type MenuOption } from 'naive-ui'
import { IconSmartHome } from '@tabler/icons-vue'
import { SliderMenusService } from './components'
import type { RouteRecordRaw } from 'vue-router'
import { children_routes } from '@/router/routes'

const menus = ref<MenuOption[]>([])

const collapsed = ref<boolean>(false)

const mapRouterToMenu = (routes: RouteRecordRaw[]) => {
  if (routes) {
    menus.value = routes.map((item) => {
      return {
        label: item.meta?.title ?? '',
        key: item.path,
        path: item.path,
        icon: () => h(IconSmartHome)
      }
    })
  }
}

mapRouterToMenu(children_routes)
</script>

<template>
  <div style="height: 100%; position: relative">
    <n-layout position="absolute">
      <n-layout-header
        style="height: 64px; padding: 12px 24px; display: flex; align-items: center"
        bordered
      >
        <div class="logo">
          <img src="@/assets/logo.svg" alt="" />
        </div>
      </n-layout-header>
      <n-layout has-sider position="absolute" style="top: 64px">
        <n-layout-sider
          bordered
          collapse-mode="width"
          :collapsed-width="64"
          :width="240"
          :collapsed="collapsed"
          show-trigger
          @collapse="collapsed = true"
          @expand="collapsed = false"
        >
          <SliderMenusService
            :menus="menus"
            :collapsed="collapsed"
            :collapsed-width="64"
            :collapsed-icon-size="22"
          />
        </n-layout-sider>
        <n-layout content-style="padding: 24px;">
          <router-view />
        </n-layout>
      </n-layout>
    </n-layout>
  </div>
</template>

<style lang="scss" scoped>
.logo {
  width: 200px;
  height: 100%;
  display: inline-block;
  img {
    width: 100%;
    height: 100%;
  }
}
</style>
