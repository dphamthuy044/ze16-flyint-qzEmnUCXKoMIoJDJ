最新款tauri2+vite7+pinia3仿微信/QQ电脑端聊天软件*Tauri2Chat*。

**tauri2-vue3-chat**最新原创Tauri2.8+Vite7.1+Vue3+Pinia3+ElementPlus桌面端仿QQ/微信界面聊天应用。封装高复用tauri2多窗体、自定义圆角阴影窗口/导航条。提供了**聊天、通讯录、收藏、朋友圈/短视频、我的**等板块。

![360截图20251021233301426](https://img2024.cnblogs.com/blog/1289798/202510/1289798-20251022093851214-1464181795.png)

### 运用技术

* 编辑器：vscode
* 跨平台框架：tauri2.8
* 前端技术框架：vite^7.1.10+vue^3.5.22+vue-router^4.6.3
* 状态管理：pinia^3.0.3
* 本地存储：pinia-plugin-persistedstate^4.5.0
* UI组件库：element-plus^2.11.5
* 富文本编辑器：@vueup/vue-quill^1.2.0
* 样式预处理：sass^1.93.2
* 短视频滑动插件：swiper^12.0.2

![p2](https://img2024.cnblogs.com/blog/1289798/202510/1289798-20251022094134938-705601068.gif)

![p6](https://img2024.cnblogs.com/blog/1289798/202510/1289798-20251022094246099-1474168463.gif)

### 项目框架目录

tauri2-vite7-wechat客户端聊天项目采用最新跨平台技术 tauri2.8 整合 vite7 搭建项目框架。

![360截图20251021221317391]()

![p5-1]()

> #### *Tauri2-ViteChat聊天系统已经更新到我的原创作品集，感谢支持！*
>
> [基于tauri2+vue3+element-plus桌面端仿微信exe聊天程序](https://github.com)

### tauri2.0结合vite.js搭建项目

![image]()

之前有写过一篇关于tauri2结合vite.js搭建桌面端项目、创建多窗口、自定义托盘闪烁及右键菜单。有兴趣的可以去看看。

[https://github.com/xiaoyan2017/p/18416811](https://github.com)

![p1]()

tauri2-vue3-chat实现类似QQ登录/主窗口切换，支持主题壁纸、自定义最大化/最小化/关闭按钮，新窗口打开朋友圈/短视频。聊天模块支持新开窗口预览图片/视频、拖拽图片到聊天区域。

![360截图20251021221317398]()

### 项目布局模板

![360截图20251021221317394]()

项目整体分为**左侧菜单栏+侧边栏+内容区(自定义导航条)**等板块。

![image]()

```
<template>
  <div class="vu__chatbox">
    <template v-if="!route?.meta?.isNewWin">
      <div
        class="vu__container flexbox flex-alignc flex-justifyc"
        :style="{'--themeSkin': appstate.config.skin}"
      >
        <div class="vu__layout flexbox flex-col">
          <div class="vu__layout-body flex1 flexbox" @contextmenu.prevent>
            
            <slot v-if="!route?.meta?.hideMenuBar" name="menubar">
              <MenuBar />
            slot>

            
            <div v-if="route?.meta?.showSideBar" class="vu__layout-sidebar flexbox">
              <aside class="vu__layout-sidebar__body flexbox flex-col">
                <slot name="sidebar">
                  <SideBar />
                slot>
              aside>
            div>

            
            <div class="vu__layout-main flex1 flexbox flex-col">
              <ToolBar v-if="!route?.meta?.hideToolBar" />
              <router-view v-slot="{ Component, route }">
                <keep-alive>
                  <component :is="Component" :key="route.path" />
                keep-alive>
              router-view>
            div>
          div>
        div>
      div>
    template>
    <template v-else>
      <WinLayout />
    template>
  div>
template>
```

![001360截图20251021234042701]()

![360截图20251021221809732]()

![360截图20251021222258498]()

![360截图20251021222634106]()

![360截图20251021223625471]()

![003360截图20251021235056245]()

![003360截图20251021235217013]()

![003360截图20251021235322345]()

![004360截图20251021235616445]()

![004360截图20251022000224472]()

![005360截图20251022000602230]()

![005360截图20251022000737083]()

![005360截图20251022000829133]()

![005360截图20251022000944690]()

![005360截图20251022001304385]()

![006360截图20251022001924526]()

![007360截图20251022002253010]()

![007360截图20251022002458272]()

![008360截图20251022002804519]()

![008360截图20251022003116183]()

![008360截图20251022003542343]()

![008360截图20251022003420305]()

![008360截图20251022003858385]()

![008360截图20251022005139749]()

![008360截图20251022005252085]()

![008360截图20251022005500412]()

![009360截图20251022072109849]()

![010360截图20251022072510626]()

![011360截图20251022072827066]()

### 项目入口配置main.js

```
import { createApp } from 'vue'
import './style.scss'
import App from './App.vue'

// 引入组件库
import VEPlus from 've-plus'
import 've-plus/dist/ve-plus.css'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

// 引入路由/状态配置
import Router from './router'
import Pinia from './pinia'

createApp(App)
.use(VEPlus)
.use(ElementPlus)
.use(Router)
.use(Pinia)
.mount('#app')
```

### tauri2+vue3自定义无边框圆角阴影窗口

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

设置 decorations: false 无边框模式。结合 transparent: true 和 shadow: false 实现自定义圆角阴影窗口。

![image]()

```
<script setup>
  import { ref } from 'vue'
  import { isTrue } from '@/utils'

  import { winSet } from '@/windows/actions'

  import Winbtns from './btns.vue'

  const props = defineProps({
    // 标题
    title: {type: String, default: ''},
    // 标题颜色
    color: String,
    // 背景色
    background: String,
    // 标题是否居中
    center: {type: [Boolean, String], default: false},
    // 是否固定
    fixed: {type: [Boolean, String], default: false},
    // 背景是否镂空
    transparent: {type: [Boolean, String], default: false},
    // 层级
    zIndex: {type: [Number, String], default: 2024},

    /* 控制Winbtn参数 */
    // 窗口是否可最小化
    minimizable: {type: [Boolean, String], default: true},
    // 窗口是否可最大化
    maximizable: {type: [Boolean, String], default: true},
    // 窗口是否可关闭
    closable: {type: [Boolean, String], default: true},
    // 关闭前回调，会暂停实例关闭 function(done)，done用于关闭
        beforeClose: Function
  })
script>

<template>
  <div class="ev__winbar" :class="{'fixed': fixed || transparent, 'transparent': transparent}">
    <div class="ev__winbar-wrap flexbox flex-alignc vu__drag">
      <div class="ev__winbar-body flex1 flexbox flex-alignc">
        
        <div class="ev__winbar-left"><slot name="left" />div>
        
        <div class="ev__winbar-title" :class="{'center': center}">
          <slot name="title">{{title}}slot>
        div>
        
        <div class="ev__winbar-extra vu__undrag"><slot name="extra" />div>
      div>
      <Winbtns :color="color" :minimizable="minimizable" :maximizable="maximizable" :closable="closable" :zIndex="zIndex" />
    div>
  div>
template>
```

tauri设置无边框窗口后，开启拖拽功能直接在需要拖拽的元素标签设置 data-tauri-drag-region 属性，另外tauri还支持css设置拖拽功能。

```
// 拖拽
.vu__drag {-webkit-app-region: drag;}
// 取消拖拽
.vu__undrag {-webkit-app-region: no-drag;}
```

### Tauri2+Vite7封装多开窗口

![013360截图20251022074406464]()

```
/**
 * 创建新窗口
 * @param {object} args 窗口配置参数 {label: 'home', url: '/home', width: 850, height: 610, ...}
 */
export async function winCreate(args) {
  await emit('win-create', args)
}
```

```
// 主窗口
export async function mainWindow() {
  await winCreate({
    label: 'main',
    url: '/',
    title: 'TAURI-WINCHAT',
    width: 850,
    height: 620,
    minWidth: 700,
    minHeight: 450
  })
}

// 登录窗口
export async function loginWindow() {
  await winCreate({
    label: 'main-login',
    url: '/login',
    title: '登录',
    width: 320,
    height: 420,
    resizable: false,
    maximizable: false,
    alwaysOnTop: true
  })
}

// 关于窗口
export async function aboutWindow() {
  await winCreate({
    label: 'win-about',
    url: '/win/about',
    title: '关于',
    width: 320,
    height: 350,
    minWidth: 320,
    minHeight: 350,
    maximizable: false,
    alwaysOnTop: true,
  })
}

// 设置窗口
export async function settingWindow() {
  await winCreate({
    label: 'win-setting',
    url: '/win/setting',
    title: '设置',
    width: 550,
    height: 470,
    resizable: false,
    maximizable: false,
  })
}
```

**自定义窗口配置参数**

```
// 系统窗口参数(与tauri的new WebviewWindow()参数一致)
const windowBaseOptions = {
  // 窗口唯一标识label
  label: null,
  // 窗口标题
  title: '',
  // 窗口路由地址
  url: '',
  // 宽度
  width: 850,
  // 高度
  height: 620,
  // 最小宽度
  minWidth: null,
  // 最小高度
  minHeight: null,
  // 窗口x坐标
  x: null,
  // 窗口y坐标
  y: null,
  // 窗口居中显示(当设置x/y，则需要设置为false)
  center: true,
  // 是否可缩放
  resizable: true,
  // 是否可最小化
  minimizable: true,
  // 是否可最大化
  maximizable: true,
  // 是否可关闭
  closable: true,
  // 最大化窗口
  maximized: false,
  // 父窗口句柄label
  parent: null,
  // 窗口是否置顶
  alwaysOnTop: false,
  // 是否显示窗口边框(要创建无边框窗口，将decorations参数设置为false)
  decorations: false,
  // 是否透明窗口
  transparent: true,
  // 是否显示窗口阴影
  shadow: false,
  // 创建时是否显示窗口
  visible: false,
  // 禁止系统拖放
  dragDropEnabled: false
}
```

### tauri2+vue3自定义托盘闪烁|消息提醒|托盘菜单

![012360截图20251022073317041]()

![012360截图20251022073147132]()

![48b25ff48ccefb11103ab70a93001ad1_1289798-20240928105906610-113063713]()

```
/**
 * 自定义托盘图标
 */

use tauri::{
  tray::{MouseButton, TrayIconBuilder, TrayIconEvent}, Emitter, Manager, Runtime
};

pub fn tray_create(app: &tauri::AppHandle) -> tauri::Result<()> {
  let _ = TrayIconBuilder::with_id("tray")
    .tooltip("TAURI-WINCHAT")
    .icon(app.default_window_icon().unwrap().clone())
    .on_tray_icon_event(|tray, event| match event {
      TrayIconEvent::Click {
        id: _,
        position,
        rect: _,
        button,
        button_state: _,
      } => match button {
        MouseButton::Left {} => {
          let windows = tray.app_handle().webview_windows();
          for (key, value) in windows {
            println!("点击左键: {}", key);
            if key == "main-login" || key == "main" {
              value.show().unwrap();
              value.unminimize().unwrap();
              value.set_focus().unwrap();
            }
          }
        }
        MouseButton::Right {} => {
          println!("点击右键");
          tray.app_handle().emit("tray_contextmenu", position).unwrap();
        }
        _ => {}
      },
      TrayIconEvent::Enter {
        id: _,
        position,
        rect: _,
      } => {
        println!("鼠标滑过托盘");
        tray.app_handle().emit("tray_mouseenter", position).unwrap();
      }
      TrayIconEvent::Leave {
        id: _,
        position,
        rect: _,
      } => {
        println!("鼠标离开托盘");
        tray.app_handle().emit("tray_mouseleave", position).unwrap();
      }
      _ => {}
    })
    .build(app);
  Ok(())
}
```

![p7]()

![image]()

```
/**
 * Tauri自定义系统托盘右键菜单
 */

import { WebviewWindow } from '@tauri-apps/api/webviewWindow'
import { LogicalPosition } from '@tauri-apps/api/window'
import { listen } from '@tauri-apps/api/event'

import { authState } from '@/pinia/modules/auth'

export default async function TrayContextMenu() {
  console.log('create tray contextmenu...')

  const authstate = authState()

  // 右键菜单宽度
  let menuW = 150
  // 右键菜单高度
  let menuH = authstate.authorization ? 300 : 48

  let webview = new WebviewWindow('win-traymenu', {
    url: '/tray/contextmenu',
    title: '托盘右键菜单',
    width: menuW,
    height: menuH,
    x: window.screen.width,
    y: window.screen.height,
    skipTaskbar: true,
    transparent: true,
    shadow: false,
    decorations: false,
    center: false,
    resizable: false,
    alwaysOnTop: true,
    focus: true,
    visible: false
  })

  await webview.listen('tauri://window-created', async() => {
    const win = await WebviewWindow.getByLabel('win-traymenu')
    win.hide()
  })
  await webview.listen('tauri://blur', async() => {
    const win = await WebviewWindow.getByLabel('win-traymenu')
    win.hide()
  })
  await webview.listen('tauri://error', async(error) => {
    console.log('traymenu error!', error)
  })

  // 监听托盘右键菜单事件
  listen('tray_contextmenu', async(event) => {
    console.log('tray_contextmenu: ', event)

    let position = event.payload
    const win = await WebviewWindow.getByLabel('win-traymenu')
    if(!win) return

    win.setAlwaysOnTop(true)
    win.setFocus()
    win.setPosition(new LogicalPosition(position.x - 5, position.y - menuH + 5))
    win.show()
  })
}
```

以上就是tauri2+vite7+vue3搭建桌面端聊天项目的一些知识分享，希望对大家有所帮助！

**附上几个最新版项目案例**

> [Electron38-Vue3OS客户端OS系统|vite7+electron38+arco桌面os后台管理](https://github.com/xiaoyan2017/p/19136187)
>
> [electron38-admin桌面端后台|Electron38+Vue3+ElementPlus管理系统](https://github.com/xiaoyan2017/p/19116145)
>
> [Electron38-Wechat电脑端聊天|vite7+electron38仿微信桌面端聊天系统](https://github.com/xiaoyan2017/p/19088804)
>
> [Vite7网页版聊天|Vue3.5+Pinia3+ElementPlus仿微信网页端web聊天系统](https://github.com/xiaoyan2017/p/19074693)
>
> [uniapp-vue3-os手机oa系统|uni-app+vue3跨三端os后台管理模板](https://github.com/xiaoyan2017/p/19054951):[黑猫加速器](https://heimaojiasuqi.com)
>
> [最新版uni-app+vue3+uv-ui跨三端仿微信app聊天应用【h5+小程序+app端】](https://github.com/xiaoyan2017/p/19031695)
>
> [Flutter3-MacOS桌面OS系统|flutter3.32+window\_manager客户端OS模板](https://github.com/xiaoyan2017/p/19018641)
>
> [最新研发flutter3.27+bitsdojo\_window+getx客户端仿微信聊天Exe应用](https://github.com/xiaoyan2017/p/18995132)
>
> [最新版uniapp+vue3+uv-ui跨三端短视频+直播+聊天【H5+小程序+App端】](https://github.com/xiaoyan2017/p/18962574)
>
> [Uniapp-DeepSeek跨三端AI助手|uniapp+vue3+deepseek-v3流式ai聊天模板](https://github.com/xiaoyan2017/p/18853514)
>
> [vue3-webseek网页版AI问答|Vite6+DeepSeek+Arco流式ai聊天打字效果](https://github.com/xiaoyan2017/p/18795796)

![duitang]()
