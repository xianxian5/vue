### vue-cli打包到服务器上问题
1. 空白
```
在config文件夹中index.js
module.exports = {
  build: {
    env: require('./prod.env'),
    index: path.resolve(__dirname, '../dist/index.html'),
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    productionSourceMap: true,


assetsPublicPath默认的是  ‘/’  也就是根目录。而我们的index.html和static在同一级目录下面。  所以要改为  ‘./ ’


```
2.vue-cli项目webpack打包后iconfont文件路径问题解决
```
webpack.base.conf.js中
module: {
    rules: utils.styleLoaders({
      sourceMap: config.build.productionSourceMap,
      extract: true,
      usePostCSS: true
    })
  },
其中extract: true,改成false
```
