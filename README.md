## Vue2.0 仿饿了么 APP
## 项目截图

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012218193325.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMjA5OTkx,size_16,color_FFFFFF,t_70#pic_left=365x650)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122180527653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMjA5OTkx,size_16,color_FFFFFF,t_70#pic_left=350x600)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190122182031627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMjA5OTkx,size_16,color_FFFFFF,t_70#pic_left=365x650)

------------------------------------------------------------------------------------------------------------------------------
## 主要技术栈：
- Vue.js
- vue-cli 脚手架
- vue-router 实现路由切换
- axios进行数据请求
- webpack打包项目文件
- ES6
- flex 弹性布局
- Vue过渡动画
- better-scrll实现联动
- localStorage本地缓存
- ----------------------------------------------------------------------------------------------------------------------------
## 实现功能
### 头部（header）
  - 点击商品，评论，商家分别跳转到对应的goods,ratings,seller界面
  - 折叠优惠信息和公告
### 商品页（goods）
   - 点击左侧menu，右侧list对应跳转到相应位置
   - 点击list查看商品详情页
   - 购物车组件添加删除商品并计算价格，点击购物车展示已选择的商品
### 评论页（ratings）
   - 商家综合评分，用户评价内容，筛选进行查看
### 商家页（seller）
   - 商家信息，公告，活动，实景图片可左右滑动，收藏 locaStorage缓存商家信息
------------------------------------------------------------------------------------------------------------------------------
## 实现功能
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012217340340.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMjA5OTkx,size_16,color_FFFFFF,t_70)


------------------------------------------------------------------------------------------------------------------------------
## 项目结构
 
```
 1.common/---- 文件夹存放的是通用的css和fonts
 2.components/---- 文件夹用来存放 Vue 组件
 3. router/---- 文件夹存放的是vue-router相关配置（linkActiveClass,routes注册组件路由）
 4. build/---- 文件是 webpack 的打包编译配置文件
 5. config/---- 文件夹存放的是一些配置项，比如我们服务器访问的端口配置等
 6.dist/---- 该文件夹一开始是不存在，在项目经过 build 之后才会生成
 7. rod.server.js---- 该文件是测试是模拟的服务器配置，用来运行dist里面的文件，在config/index.js中,build对象中添加一条端口设置port：8080，
 8.App.vue---- 根组件，所有的子组件都将在这里被引用，
 9.index.html---- 整个项目的入口文件，将会引用我们的根组件 App.vue
 10.main.js---- 入口文件的 js 逻辑，在 webpack 打包之后将被注入到 index.html 中

```
