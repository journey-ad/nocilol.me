---
title: 提取ArtStation自带模型查看器中的模型数据
date: 2019-11-02 20:52:00
categories: lab
tags: [3D,破解,模型]
urlname: ripping-artstation-models
---
![3D Fanart Wataten: An Angel Flew Down to Me](https://img.imjad.cn/images/2019/11/03/an_angel_flew_down_to_me.jpg)

{% cplayer %}
- 1356233927
- 1356237497
- 1356237499
- 1356233951
- 1356237498
- 1356237501
- 1356237500
- 1356233953
- 1356233949
{% endcplayer %}

## 前言
>需要说明的是，折腾仅限用于学习用途，可通过正规途径购买的模型请勿非法提取，请勿对模型、贴图及动画等数据进行修改、二次分发、用于商用等违反[ArtStation服务条款](https://www.artstation.com/tos)及著作权相关法律法规的行为。使用本文提到的方法提取模型产生的一切后果由操作人承担，与本网站及本人无关



没想到时隔大半年的首次更新还是因为萝莉🤪，翻看评论列表时看到[此前介绍Sketchfab模型提取的文章](https://imjad.cn/archives/lab/ripping-sketchfab-models)的评论中有人提到~~而我因为太懒并没有回复他😪~~智乃模型的作者Xkung目前转到ArtStation发布作品，并且已经发布了[新作](https://www.artstation.com/artwork/3oLez2)—— 动画「<span style="color:#E12351">天<span style="color:#A35E95;">使</span><span style="color:#6BAE23;">降</span><span style="color:#00B9FA;">临</span><span style="color:#FC951C;">到了</span>我<span style="color:#FC951C;">身边</span>！</span>」中的五位小学生。于是乎又一次「顿时感觉想要拥有」，同样的，模型作者并没有开放下载和购买，那么魔高一丈，就来找ArtStation的提取方法吧

## 提取

打开ArtStation的作品页面，观察查看模型前后浏览器开发者工具的资源请求，很明显，有一个`.mview`结尾的请求，看文件大小是包含模型的数据了，并且就加载完成后贴图文件也瞬间以Object URL的形式加载完毕，说明贴图文件也包含在此mview文件中。在新标签页打开会跳转下载

![network log](https://img.imjad.cn/images/2019/11/03/Snipaste_2019-11-03_03-05-42.png)

用十六进制编辑器打开mview文件，发现文件内容未经过加密，似乎是直接将各个部件的二进制数据拼接起来的，而我的半吊子水平分析到这一步也基本到头了，下一步的解析需要借助前人的智慧

>「99%的问题都已经被前人遇到并解决了，而你只需要谷歌」　　　　——『飞鸟集』

让我们来搜索解析工具，好的，搜到了：[majimboo/mviewer](https://github.com/majimboo/mviewer)，根据介绍，这个脚本能将mview文件中包含的dat模型文件、贴图及json描述文件提取出来，并转换为obj格式的模型

执行命令，成功拆解出贴图和一堆obj部件，注意此步骤得到的贴图文件名是正确的原始名

```bash
# mview文件解包
python extract_mview.py <filename>

# dat转换为obj模型
python extract_model.py <folder_containing_scene.json>
```

![extract mview](https://img.imjad.cn/images/2019/11/03/Snipaste_2019-11-03_03-52-56.png)

作者同样提供了一个附带了mview解析插件的[Noesis](http://richwhitehouse.com/index.php?content=inc_projects.php)，可视化操作更加方便，并且可以导出模型整体为多种常用格式

![Noesis](https://img.imjad.cn/images/2019/11/03/Snipaste_2019-11-03_03-59-59.png)

直接将mview文件拖入Noesis，稍后将解析成功，右侧预览窗口能看到模型预览。然后依次点击File - Export进行导出操作，导出格式选择obj或fbx，其余保持默认，一段时间后将导出完成，包含模型本体与一堆被重命名的贴图文件

## 修正贴图轴向

将导出后的模型文件拖入3dmax，发现模型内贴图名为原始文件名，新建`textures`目录，将手动执行脚本得到的原始贴图放入，发现3dmax已经能识别到贴图，但uv方向似乎有些问题，调整u方向为180度，即y轴翻转后显示正常

![调整方向](https://img.imjad.cn/images/2019/11/03/Snipaste_2019-11-03_02-15-45.png)

![不然就会这样](https://img.imjad.cn/images/2019/11/03/Snipaste_2019-11-02_03-33-04.png)

但挨个调整还是过于麻烦，搜索一番后，找到了批量调整贴图的工具：[GD Ripper](https://tieba.baidu.com/p/4696461174)，看介绍是一套用于手游3D模型提取的工具集，有机会可以研究研究。这里用到的是「高级工具-贴图工厂-图片旋转翻转」的功能，根据打印出的日志来看，实质是调用的[NConvert](https://newsgroup.xnview.com/viewforum.php?f=57)进行的转换

![翻转y轴](https://img.imjad.cn/images/2019/11/03/Snipaste_2019-11-03_04-20-55.png)

完成之后再次导入至3dmax，显示已经正常，至此整个提取导入的操作全部完成

## 移除不必要的高光与描边

在左侧对象列表中选中所有包含**line**与**hilight**字样的对象，直接删除即可，可使用高级过滤器快速筛选

![Fillter](https://img.imjad.cn/images/2019/11/03/Snipaste_2019-11-03_02-04-35.png)

## 相关链接

[ArtStation - 3D Fanart Wataten : An Angel Flew Down to Me, Xkung Work](https://www.artstation.com/artwork/3oLez2)

[majimboo/mviewer: Reverse Engineer MView 3D File Format](https://github.com/majimboo/mviewer)

[手游3D模型提取工具《GD Ripper V2》【gdstudio吧】\_百度贴吧](https://tieba.baidu.com/p/4696461174)