---
title: Markdown编辑器 HyperMD for Typecho
date: 2018-06-21 05:17:00
categories: code
tags: [typecho,插件,plugin,markdown,编辑器]
urlname: hypermd-for-typecho
---
![demo](https://img.imjad.cn/images/2018/06/21/sp180621_152846.png)

!https://github.com/journey-ad/HyperMD-Typecho-Plugin

## 前言
[HyperMD](https://github.com/laobubu/HyperMD)是一个基于[CodeMirror](https://codemirror.net)的所见即所得MarkDown编辑器，拥有许多非常棒的特性
这个插件可以将Typecho的文章编辑器替换为HyperMD，使写作成为一种享受
正如HyperMD的Slogan所写的那样：打破Markdown*写作* 和 *预览* 的 **次元壁**！


## 安装
前往本插件的[发布页](https://github.com/journey-ad/HyperMD-Typecho-Plugin)，点击**Download ZIP**按钮，解压，将`HyperMD-Typecho-Plugin-master`重命名为 `HyperMD` ，之后上传到你博客中的`/usr/plugins`目录，在后台启用即可

## 特性
*此处搬运自HyperMD的说明文档，经过部分修改*

1. 写作和预览，一个框就够了
   - **常规Markdown语法**
     + **粗体字**、*斜体字*、~~删除线~~、`代码`
     + [链接](https://imjad.cn)、图片
     + 标题/引用块/代码块/列表/水平分割线
   - **扩展 Markdown 语法**
     + 简单绘制表格
     + 脚注 [^1]
     + YAML Front Matter
   - 行内**LaTeX**公式渲染，支持多行公式块 [^3]
2. **按着Alt并点击** 可以打开链接，或者跳到脚注 [^1]
3. **代码块语法高亮** 支持数百种语言 [^2]
4. **鼠标悬停** 可以查看脚注内容
5. **复制粘贴** 自动转换网页内容为Markdown [^4]
6. **上传图片** 只需要复制粘贴，或者把文件拖进来就行了 [^5]
7. **KeyMap** 方便的快捷键
    + **表格**
      - `回车` 使用 `| column | line |` 创建表格
      - `回车` 插入一行或者结束表格（如果最后一行的格子都是空的）
      - `Tab` 和 `Shift-Tab` 在表格间切换
    + **列表**
      - `Tab` 和 `Shift-Tab` 改变当前列表项的缩进
    + **格式化** 光标旁边的单词（或者选中的文字）
      - `Ctrl+B` **加粗**
      - `Ctrl+I` *斜体*
      - `Ctrl+D` ~~删除线~~

[^1]: 按着 Ctrl 点击也行
[^2]: 只要是 CodeMirror 支持的都能用
[^3]: 公式块和代码块类似， 使用 `$$` 包裹你的公式，支持多行，正文中显示需要MathJax引擎支持
[^4]: 使用 `Ctrl+Shift+V` 可以粘贴纯文本
[^5]: 使用[SM.MS](https://sm.ms)提供的服务