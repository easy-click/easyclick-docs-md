---
title: EasyClick安卓文档_安卓手机自动化脚本_节点图色抓取
hide_title: false
hide_table_of_contents: false
sidebar_label: 节点工具面板
description: 'EasyClick手机自动化脚本,使用idea设置EasyClick的节点抓取，手机截图，图色生成'
keywords:
  - EasyClick
  - 手机自动化脚本
  - 自动化软件
  - EasyClick脚本节点抓取
  - 图色生成
  - node
  - png
  - img
  - src
  - androidimg
  - alt
  - style
  - zoom
  - preview
  - size
  - 手机自动化
  - 自动化测试
  - 脚本开发
---

# 节点图色抓取
    - 位置: `IDEA菜单栏-EasyClick安卓版-节点抓取`
    - <img src='/androidimg/node/node_preview.png' alt="node_preview.png" style={{zoom:'30%'}} />
    - 点击节点面板上的`齿轮`，选择`View Mode - Float`, 即可将节点面板独立出来
    - <img src='/androidimg/node/node_size.png' alt="node_size.png" style={{zoom:'30%'}} />

## 获取节点
    - 点击 `获取节点` 按钮，可以获取当前的屏幕元素，前提是手机上打开了`自动化服务`
        - 详细的日志输入，一定要看`EasyClick安卓版运行日志`的日志
    - 抓节点成功的面板:
    - <img src='/androidimg/node/node_ok.png' alt="node_ok.png" style={{zoom:'30%'}} />

## 节点文件列表
    - <img src='/androidimg/node/node_filelist.png' alt="node_filelist.png" style={{zoom:'30%'}} />
    - 在`左侧文件列表-搜索框输入文字`可以进行文件搜索
    - 节点文件都是以`当前运行APP包名+日期`的形式命名
    - 在`左侧文件列表点击鼠标右键`,可以`刷新文件`、`删除文件`、`修改文件名称`

## 图片视图
    - <img src='/androidimg/node/node_img.png' alt="node_img.png" style={{zoom:'30%'}} />
    - 中间图片，使用鼠标左键在上面点击，系统会自动使用红色框，选中你点击的区域，同时右侧的节点树形也会跟着选中  
    - 鼠标右键可以复制颜色值等等

## 节点属性
    - <img src='/androidimg/node/node_tree.png' alt="node_tree.png" style={{zoom:'30%'}} />
    - 右侧节点属性面板，可以在输入框填写`关键字`，点击上下剪头进行查找
    - 点击`树形结构`的每个节点，进行定位查看详细属性
    - 双击`属性与值`面板，可以复制`ctrl+a`快捷键复制值
    - 按住ctrl键，选择属性，然后通过鼠标右键菜单，进行`生成查找节点的代码`

## 其他操作
    - `保存节点&截图`，保存当前选打开的节点和图片
    - `载入节点&截图`，载入电脑中的节点和图片
    - `单Tab模式`，选中后，每次打开文件列表的节点，都会覆盖已打开的，否则就会是新窗口打开

