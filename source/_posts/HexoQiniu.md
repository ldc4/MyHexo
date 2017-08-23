title: 为Hexo博客使用hexo-qiniu-sync插件[多图慎入]
date: 2016-03-09 09:56:53
categories:
- Plugin
tags:
- qiniu
- plugin
- hexo
---
hexo-qiniu-sync插件：使用七牛为Hexo博文存储图片，加快访问速度。

高能：废弃！！！！
理由：在从我的笔记本移植到我司的主机上，由于这个插件导致我的博客停更一个月！！！

<!--more-->
### 序言
之前某凯给我发了一个插件hexo-qiniu网站，待我装上，然后运行，直接报错。
然后我细心一看，特码2年没维护了，而且功能就一个，帮你自动生成标签，还不带同步上传的。
然后我就另外找了一个，其实一共就三个。
- hexo-qiniu  
- hexo-qiniu-images
- hexo-qiniu-sync

废话不多说，直接搬：


### 安装hexo-qiniu-sync插件

#### 在你的hexo主目录下运行以下命令进行安装
``` bash
npm install hexo-qiniu-sync --save
```

#### 添加插件配置信息到 _config.yml 文件中
``` bash
plugins:
  - hexo-qiniu-sync
#七牛云存储设置
##offline       是否离线. 离线状态将使用本地地址渲染
##sync          是否同步
##bucket        空间名称.
##access_key    上传密钥AccessKey
##secret_key    上传密钥SecretKey
##dirPrefix     上传的资源子目录前缀.如设置，需与urlPrefix同步 
##urlPrefix     外链前缀. 
##local_dir     本地目录.
##update_exist  是否更新已经上传过的文件(仅文件大小不同或在上次上传后进行更新的才会重新上传)
##image/js/css  子参数folder为不同静态资源种类的目录名称，一般不需要改动
##image.extend  这是个特殊参数，用于生成缩略图或加水印等操作。具体请参考http://developer.qiniu.com/docs/v6/api/reference/fop/image/ 
##              可使用基本图片处理、高级图片处理、图片水印处理这3个接口。例如 ?imageView2/2/w/500 即生成宽度最多500px的缩略图
qiniu:
  offline: false
  sync: true
  bucket: bucket_name
  access_key: AccessKey
  secret_key: SecretKey
  dirPrefix: static
  urlPrefix: http://bucket_name.qiniudn.com/static
  local_dir: static
  update_exist: true
  image: 
    folder: images
    extend: 
  js:
    folder: js
  css:
    folder: css
```
一些参数说明，具体请参考[hexo-qiniu-sync插件官方](http://cnpmjs.org/package/hexo-qiniu-sync)

### 同步图片到七牛
你只需要将图片放在:local_dir/:image中，根据你填的local_dir和image参数
如果你本地跑着hexo server，它也会提示你的:Please run `hexo qiniu sync` to sync.
放好图片后，就在CMD中执行：
``` bash
hexo qiniu sync
```

### 使用标签
``` bash
{% qnimg imageFile attr1:value1 attr2:value2 'attr3:value31 value32 value3n' [extend:?imageView2/2/w/600 | normal:yes] %}
{% qnjs jsFile attr1:value1 attr2:value2 'attr3:value31 value32 value3n' %}
{% qncss cssFile attr1:value1 attr2:value2 'attr3:value31 value32 value3n' %}
```
例如 ：
``` bash
{% qnimg HexoQiniu/hexo-qiniu-1.png extend:-small%}
```

### 处理图片
这样就算是能用了。但是放上来的图片大小不一，就得用样式/接口来处理一下
#### 用七牛样式处理
>  1. 首先登陆七牛空间，选择存储空间后，再选择`数据处理`菜单。
>  2. 设置分隔符。默认的 `-` 即可。
>  3. 点击 `新建样式` 按钮，根据提示创建一个处理样式。
>  4. 创建样式完毕后，你就可以将 `extend` 参数设置为 `分隔符+样式名称`了。  

如你设置的分隔符为 `-` ，样式名称为 `small` ，则 `extend` 参数就是 `-small` 了。  
简单吧？  
你可以根据自己的需要，建立多个样式，然后在文章内使用时，为不同图片标签设置
不同的`extend`参数，来达到不同的显示效果。

#### 用七牛imageView2接口处理
上一种处理方式处理后图片的后缀会变，如：.png-small。fancybox就用不了了
因此，可以使用[imageView2接口](http://developer.qiniu.com/code/v6/api/dora-api/index.html#imageview2),具体请跳转翻阅
``` bash
{% qnimg test/demo.png title:图片标题 alt:图片说明 'class:class1 class2' extend:?imageView2/2/w/600 %}
```


#### 修改模板样式处理
这里仅仅针对spfk模板，至于模板的img样式在哪，需要参考你使用的模板的文件结构
修改themes\spfk\source\css\_partial\artical.styl的img样式，对全局的img进行样式显示修改
``` bash
.article {
  margin: 30px;
  position: relative;
  -webkit-transition: all 0.2s ease-in;
  &.show{
    visibility: visible;
    -webkit-animation: cd-bounce-1 0.6s;
    -moz-animation: cd-bounce-1 0.6s;
    animation: cd-bounce-1 0.6s;
  }
  &.hidden{
    visibility: hidden;
  }
  img{
    max-width: 900px; /* max-width: 100%; */
  }
}
```

前2种方式是直接对图片处理，后一种方式是对图片的显示处理

### 小技巧
>如果你经常在文章内插入图片，你可以修改文章模板，将空白的图片插入标签粘贴进去。  
这样新建立的文章就有空白标签可以让你直接填写图片路径就好了，会很省事。  
文章模板文件：`./scaffolds/post.md`  
图片标签
``` bash
{% qnimg test/demo.png title:图片标题 alt:图片说明 'class:class1 class2' %} 
```

### 问题
alt属性生成的是title,title style貌似没有用

### 测试hexo-qiniu-sync

之前我在做的时候的过程图：
![nimm](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-1.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-2.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-3.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-4.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-5.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-6.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-7.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-8.png)
![](http://ldc4.qiniudn.com/images/HexoQiniu/hexo-qiniu-9.png)


