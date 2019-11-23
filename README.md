# ♬ 原生小程序云开发 ---- 仿网易云音乐 ♪

这是云开发的快速启动指引，其中演示了如何上手使用云开发的三大基础能力：

- 数据库：一个既可在小程序前端操作，也能在云函数中读写的 JSON 文档型数据库
- 文件存储：在小程序前端直接上传/下载云端文件，在云开发控制台可视化管理
- 云函数：在云端运行的代码，微信私有协议天然鉴权，开发者只需编写业务逻辑代码

## 0️⃣  运行部署到自己电脑中运行

① 首先可以的话，可以Fork一下我的项目到自己的Github，项目还在更新后续可以方便查看文档，然后把我的代码下载下来解压到自己电脑

② 注册个小程序是第一步（首次注册可以看[官网文档](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/getstart.html#申请帐号)），然后将项目在微信开发者工具打开，接下来迫在眉睫的当然就是部署服务器（也就是创建自己的云服务）

③ 在微信开发者工具中，点击左上角的【云开发】进去创建云服务界面，设置好之后，返回大概等待20-30min中后重新启动微信开发工具，此时点击【编译】就可以成功对接云



![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/deploy_01.png)

④ 然后你将 `cloudfuncitons` 下的几个云函数，右键终端打开 `npm install` 一下 ，因为这些云函数中有些用到了其他库的依赖（你也可以直接在cloudfunctions顶层云函数文件夹中选择终端，然后 `npm install`），有些小伙伴会报错比如没有权限或者没有识别到 ·npm· 这个指令，问题就是你需要在你电脑全局安装一个Node@8+，此时也会同时带有npm的，如果还是不行就是权限问题（Window / Mac），你需要以管理员的身份调出你的终端进行操作；（如果嫌麻烦的还可以在自己电脑装一个 [git Bash](https://git-scm.com/download/)）

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/deploy_02.png)

⑤ 项目刚起步，用到了一个云数据库表，所以你进去你【云开发】界面点击【数据库】，下面新建一个集合名称，名字要起对，叫 `playlist` → 这是歌曲的数据存放地；现在项目已经完成了音乐模块，后续还有一些优化，后面会更新，具体可看文档；【补充：】博客模块开发完成，需要再增加一个集合名称为 `blog`

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/deploy_03.png)

⑥ 【这一步可以不部置】项目中涉及图片或者视频资源的上传，需要在【云开发】界面点击【存储】，创建两个放资源的文件夹，一个是放图片的 `blog-image`，另一个是 `blog-video`；这步骤可以跳，不创建的话也会开发工具也会自动新建，当然对于低版本开发工具是否有这个功能就不知道了，所以最好升级最新版本的开发工具，否则就按上面操作就行

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/cloud-save.png)



⑦ 后续随着项目功能模块增加，云服务那边的部署就会复杂一点，但我会在这里说明好的，按照上面来应该没有问题，欢迎  :sparkles::sparkles::sparkles: star​ :sparkles::sparkles::sparkles:

------



## 1️⃣  参考文档

- [云开发文档](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/getting-started.html)

## 2️⃣  项目进度

### 2️⃣ . 1️⃣  音乐模块开发

- cloudfunciton

1. 实现定时触发器云函数的定时周期爬数据（网易云音乐）歌单及其歌曲数据
2. 使用tcb-router管理项目路由跳转部分
3. 突破小程序读取条数限制 + 配合云数据库完成数据的增删改查

- miniprogram

1. > 【完成小程序音乐模块中的轮播图，歌单的获取及展示，歌单内部歌曲的获取及展示】

   > > 1.1 swiper轮播图原生组件

   > > 1.2 组件定义开发playlist （歌单列表）、musiclist（音乐列表） 
   > >
   > > 1.3 对接网易云歌单接口进行请求展示，格式化播放量
   


![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/music-list.jpg)

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/music-list.png)

2. > 【完成音乐面板布局 + 歌曲播放以及歌词联动等细节（音乐模块完成）】

   > > 2.1 配合H5动画还原网易云音乐播放动画

   > > 2.2 组件化开发 progress-bar（播放进度条） 、lyric（歌词界面）
   
   > > 2.3 用getBackgroundAudioManager完成播放音乐功能
   
   > > 2.4 配合movable-area、movable-view、progress完成歌词播放进度条联动
   > >
   > > 2.5 组件间通信+组件生命周期 、组件和页面通信+页面生命周期
   > >
   > > 2.6 高度还原网易云音乐的播放界面细节点

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/music-player.png)

### 2️⃣ . 2️⃣  博客发现模块开发

- cloudfunciton

  1.创建博客blog集合数据库，数据由miniprogram前端部分向数据库增加字段（用户微信名称，用户微信头像，发布的图片或者视频的FileID，发布的内容，发布的服务端时间）

  2.发布的图片和视频将存储在云存储中（图片blog-image，视频blog-video），前端页面通过fileID进行获取

  3.使用微信小程序创建索引管理，对涉及搜索的字段进行优化慢查询操作（针对content发布内容，根据服务器时间publishTime排序）

  ![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/blog-search.png)

- miniprogram

1. > 【完成小程序博客模块发布界面的授权登录】

   > > 1.1 组件化两个 author-popup（授权登录弹窗），author（授权登录）

   > > 1.2 完成组件突破组件隔离使用全局样式

   > > 1.3 使用button中open-type为getUserInfo进行用户信息获取，通过wx.getSetting拿到对应授权信息

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/blog-author.png)



2. > 【博客模块发布编辑页制作（测试：【真机调试】，开发工具部分功能测不了，本人使用IOS测，Android没测多少有BUG可以私我）】

   > > 2.1 编辑字数检测，监测不同机型拉起键盘高度改变发布底部发布布局样式

   > > 2.2 可以根据用户需求上传图片或者视频，默认最多9，视频1

   > > 2.3 点击图片或者视频都可以进入预览图片或者播放视频，点击右上角的叉叉可进行删除

   > > 2.4 解决BUG：修复播放视频时，textarea输入框（原生组件）遮住播放视频的返回编辑页的按钮；增加切换发布类型（图片 / 视频）仍可以记录上一个编辑的最后格式

图片不展示那么多，可以自己部署本地，自己真机测试，有什么缺点或者优化可以私我~

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/publish-type.jpg)

3. > 【将用户图片或者视频上传云存储，将博客展示卡片需要的内容信息添加到云数据库中】

   > > 3.1 检查用户发布的内容是否符合发布规则（发布文字和图片 / 视频 两者必有其一方可进行发布操作）

   > > 3.2 使用 `wx.cloud.uploadFile` 将图片或者视频资源上传到云存储

   > > 3.3 链接数据库集合 【blog】 ，添加博客卡片需要的数据（用户名，头像，发布的 图片/视频 地址链接，发布文案，及其发布时间）

   

4. > 【完成博客卡片列表 + 模糊搜索功能】

   > > 4.1 从发布成功到展示博客列表，完成自动查询数据库并展示发布内容

   > > 4.2 增加一个PublishType字段用于区分发布的是图片还是视频，方便做展示处理

   > > 4.3 继承了发布编辑页的功能，在博客列表点击图片或者视频都可以预览或者播放

   > > 4.4 点击博客卡片可以进入博客评论页（开发中...）
   
   > > 4.5 配合云数据库进行模糊搜索并展示对应搜索结果的博客列表

![image](https://github.com/Umbrella001/wx-yunyinyue/raw/master/DocImage/blog-card.jpg)
