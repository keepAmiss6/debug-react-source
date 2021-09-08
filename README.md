# 如何阅读react的源码？如何在react的源码中跟断点？
- 一般我们的方法是在github上把react的源码文件下载下来，然后通过打包react、react-dom、scheduler文件生成编译后的文件；再通过创建软连接的方式链接调试
- 但是我在实践中(使用的react17.0.2版本的源码)，通过上诉的方法，一个错接着一个，可能因为react的架构太庞大了吧，总有些地方不对付。
- 后来换了一种思路，在https://www.jsdelivr.com/package/npm/react?path=umd 上找到react的CDN，用它去链接调试，非常方便，具体操作如下
# 使用react CDN调试react源码
- 在https://www.jsdelivr.com/package/npm/react?path=umd下载对应的包
- 创建自己的react项目
  - npx create-react-app react-demo
  - cd react-demo
  - npm install react-app-rewired --save-dev
- 在根目录文件夹下新建 config-overrides.js 文件，配置 webpack 的 externals 属性，让项目内的 react、react-dom 都能够走 window 下挂载的对象
```
/* config-overrides.js */

module.exports = function override(config, env) {
    // do stuff with the webpack config...
    config.externals = {
        'react': 'window.React',
        'react-dom': 'window.ReactDOM',
    };
    return config;
}
```
- react 挂载到 window 上操作：把我们之前在 CDN 上下载的 develope 版的源码放到 public 目录，然后在 public/index.html 中引入源码
```
<script src="./umd/react.development.js"></script>
```
- 启动项目 npm run start (react-scripts start)

![image.png](http://ww1.sinaimg.cn/large/006SZA09ly1gtviavooslj61pg108b0t02.jpg)

撒花，这样我们就可以愉快地在react源码中跟断点啦～～～




