---
title: 破解Sketchfab加密并提取模型数据
date: 2018-11-20 15:52:00
categories: lab
tags: [3D,破解,模型]
urlname: ripping-sketchfab-models
---
![魔法少女智乃](https://img.imjad.cn/images/2018/11/20/record.gif)

{% cplayer %}
- 33955999
{% endcplayer %}

## 前言
>需要说明的是，折腾仅限用于学习用途，可通过正规途径购买的模型请勿非法提取，请勿对模型及贴图等数据进行修改、二次分发、商用等违反Sketchfab版权规定及著作权相关法律法规的行为。使用本文提到的方法提取模型产生的一切后果由操作人承担，与本网站及本人无关



最近在Sketchfab看到了一个[智乃的模型](https://sketchfab.com/models/9120703a4aee4c2cb0313a9ca3e1e1a3)，顿时感觉想要拥有，但模型作者并没有开放下载和购买，迫于实在是心痒难耐，便在网上寻找破解Sketchfab加密的方法

首先在某宝上搜索，发现有几家店铺提供了收费下载服务，虽然没有明说，但看描述信息可以实现“任意下载”，心中底气又足了两分~~某宝下载服务均价20 RMB，希望不要因为砸了别人饭碗而被追杀🌝~~

网络上的资料不多，零星几个提到的信息指向了Ninja Ripper。这一工具原理是劫持系统DirectX接口，直接从内存中拿到渲染数据，理论上可以支持任意D3D应用，并且无视任何加密，毕竟不管怎么加密，最后系统喂给显卡的都是标准的数据。可惜的是Sketchfab在几个月前已经升级了系统，提取出来的都是一些残片，没有使用价值

抱着不死心的想法继续搜索，终于在一个毛子论坛上找到了希望：一个解密脚本。这个工具的原理就很直接了，因为Sketchfab使用了WebGL来显示模型，那么理论上就一定能够破解，毕竟加解密算法都在前端。这个脚本的作者逆向了出解密算法，并用Python实现出解密工具，做成了Blender的插件，虽然都是很古老的版本，但要什么自行车，能用就行

那么就利用空余时间，简单记下折腾过程


## 提取

### 所需工具
- Python 2.6.6
- Blender 2.49
- **[Blender249[osgjs].zip](https://cdn.imjad.cn/usr/uploads/Blender249[osgjs].zip)** 解密脚本


### 工具准备
1. 安装Python环境 (以下方法二选一，推荐a方法，可移动使用且不影响其他Python版本)
   - a.下载Python 2.6.6源码包，将其中的**Lib**目录中的内容压缩为**python26.zip**并放置于Blender安装目录
   - b.执行安装程序并添加环境变量`PythonPath`，值为`C:\Python26;C:\Python26\DLLs;C:\Python26\Lib;C:\Python26\Lib\lib-tk`（假设安装目录为`C:\Python26`）
2. 安装Blender 2.49
3. 将**Blender249[osgjs].zip**中的**newGameLib**目录解压至Blender安装目录中的`.blender\scripts`目录下
4. **Blender249.blend**单独放置


### 下载模型数据
1. 打开浏览器并打开Dev Tools
2. 打开所需模型的Sketchfab页面，确保模型已经完全加载
3. 在Network选项卡中找到如下两个文件并下载下来放到一起
   - **file.osgjs.gz**
   - **model_file.bin.gz**
4. 其中**file.osgjs.gz**是一个JSON文件，将其重命名为**file.osgjs**
5. 贴图资源在`media.sketchfab.com`下，有不同分辨率之分，注意鉴别
6. 也可以忽略以上步骤，直接从网页包含的数据中提取，具体可参考下列代码：


#### 模型数据下载脚本
为了快速下载模型数据，我写了一个简单的下载脚本：
```python
import os
import re
import json
import requests
from urllib.parse import urlparse
from html import unescape
from bs4 import BeautifulSoup

SCRIPT_VERSION = '1.0'

HEADERS = {
    'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.25 Safari/537.36',
    'accept-encoding': 'gzip, deflate, br',
    'accept-language': 'zh-CN,zh-TW;q=0.8,zh;q=0.6,en;q=0.4,ja;q=0.2',
    'cache-control': 'max-age=0'
}

def main():
    url = input('input url:')
    parse(url)

def parse(url):
    try:
        print('Parsing...')
        page = requests.get(url, headers=HEADERS, timeout=10).text
        soup = BeautifulSoup(page, 'html.parser')

        modelId = urlparse(url).path.split('/')[2].split('-')[-1]
        data = unescape(soup.find(id='js-dom-data-prefetched-data').string)
        data = json.loads(data)
        name = validateTitle(data['/i/models/'+modelId]['name'])
        thumbnailData = data['/i/models/'+modelId]['thumbnails']['images']
        thumbnail = getBiggestImage(thumbnailData)
        osgjsUrl = data['/i/models/'+modelId]['files'][0]['osgjsUrl']
        modelFile = osgjsUrl.replace('file.osgjs.gz', 'model_file.bin.gz')
        texturesData = data['/i/models/'+modelId+'/textures?optimized=1']['results']
        textures = []

        print('Model Id:', modelId)
        print('Name:', name)
        print('Thumbnail URL:',thumbnail)
        print('osgjs URL:', osgjsUrl)
        print('Model File:', modelFile)
        print('Textures:', len(texturesData))

        download(thumbnail, os.path.join(name, 'thumbnail.jpg'))
        download(osgjsUrl, os.path.join(name, 'file.osgjs'))
        download(modelFile, os.path.join(name, 'model_file.bin.gz'))

        for texture in texturesData:
            textureUrl = getBiggestImage(texture['images'])
            download(textureUrl, os.path.join(name, 'texture', validateTitle(texture['name'])))

    except AttributeError:
        raise
        return False

def getBiggestImage(images):
    size = 0
    for img in images:
        if img['size'] != None and img['size'] > size:
            size = img['size']
            imgUrl = img['url']
    return imgUrl

def validateTitle(title):
    pattern = r'[\\/:*?"<>|\r\n]+'
    newTitle = re.sub(pattern, "_", title)
    return newTitle

def download(url, filename):
    print('Downloading:', filename)
    try:
        os.makedirs(os.path.dirname(filename), exist_ok=True)
        if os.path.exists(filename):
            if os.path.getsize(filename) > 0:
                print('file exists.')
            else:
                with open(filename, 'wb') as file:
                    file.write(requests.get(url, headers=HEADERS, timeout=30).content)
        else:
            with open(filename, 'wb') as file:
                file.write(requests.get(url, headers=HEADERS, timeout=30).content)
    except Exception:
        pass

if __name__ == '__main__':
    main()
```


### 提取过程
1. 启动Blender，将随之启动一个CMD窗口，可观察有无报错
2. 点击**File** - **Open**，打开`Blender249.blend`，然后按<kbd>Ctrl</kbd> + <kbd>U</kbd>，这样以后就不用重复此步了
3. 右键点击窗口左侧的Python代码区域，然后点击**Execute Script**或按<kbd>Alt</kbd> + <kbd>P</kbd>
4. 在出现的文件选择对话框中导入此前下载的`file.osgjs`
5. 如果一切正常，CMD窗口将刷过大量解密操作相关信息，并在视图中显示解密后的模型
6. 点击**File** - **Export**，将解密后的模型文件导出为其他格式
7. 贴图和动画数据不会被保存，导出的模型可以在任何3D制作软件中进行编辑

![Blender里的智乃](https://img.imjad.cn/images/2018/11/20/sp181117_203901.png)


#### 可能遇到的错误

##### 提示`IndexError: list index out of range`，完整报错类似如下：
```shell
Traceback (most recent call last):
  File "starter.py", line 1767, in openFile
  File "starter.py", line 1760, in Parser
  File "starter.py", line 1723, in osgParser
  File "C:\blender-2.49-win64\.blender\scripts\newGameLib\myLibraries\meshLib.py", line 670, in draw
  File "C:\blender-2.49-win64\.blender\scripts\newGameLib\myLibraries\meshLib.py", line 588, in addFaces
  File "C:\blender-2.49-win64\.blender\scripts\newGameLib\myLibraries\meshLib.py", line 822, in indicesToTriangleStrips
IndexError: list index out of range
```

解决方法：blender左侧的Python代码注释掉`330-351`行


## 看图
![没穿衣服的智乃](https://img.imjad.cn/images/2018/11/20/sp181117_214136.png)

![加上贴图的智乃](https://img.imjad.cn/images/2018/11/20/sp181117_214107.png)