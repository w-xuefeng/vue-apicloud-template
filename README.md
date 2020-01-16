<div align="center">
  <img width="200" src="./assets/logo.png">
  <h1>vue-apicloud-quickstart</h1> 
  <p>使用 Vue.js + APICloud 开发移动端应用的起手式</p>
</div>

[English](./README_EN.md) | [简体中文](./README.md)

## 使用方法

### 单独使用

- 安装

  ```shell
  $ npm i vue-apicloud-quickstart
  ```
- 在 main.js中 引入项目

  ```js
  import Vue from 'vue'
  import App from './App.vue'
  import VAQ from 'vue-apicloud-quickstart'
  import pages from '@/config/pages'
  // 页面配置文件，需要手动创建

  Vue.config.productionTip = false

  Vue
    .use(VAQ, {
      pages,
      debugOnPC: true // 是否开启 PC 调试，默认 false
    })
    .init({
      el: '#app',
      render: h => h(App)
    })
    // 链式调用 init 方法， 即创建 Vue 实例
    // 开启 PC 调试时与 new Vue() 等同
    // 未开启时将在 apiready 的回调中创建 Vue 实例
  ```
### 配合 vue-cli-plugin-apicloud 使用（推荐）

- 运行以下命令即可

  ```shell
  $ vue add @w-xuefeng/apicloud
  ```

## 说明

- 1 页面配置

  - 在 `src` 目录下建立 `pages` 目录，和 `config` 目录，假设现有开屏广告页 
  `index/index.vue`、登录页面 `login/index.vue` 和 应用首页 `home/index.vue`， 其目录结构如下所示：

    ```
    ├── src
    |   |
    │   ├── pages                # 页面入口
    |   |   |
    |   |   ├── index
    |   |   |   |
    |   |   |   ├── index.vue    # 开屏广告页
    |   |   |   |
    |   |   ├── login
    |   |   |   |
    |   |   |   ├── index.vue    # 登录页
    |   |   |   |
    |   |   └── home
    |   |       |
    |   |       └── index.vue    # 应用首页
    |   |
    │   └── config               # 配置入口
    |       |
    |       └── pages.js         # 页面配置文件
    ```

  - 页面配置文件 `pages.js`， 结构如下

    ```js
    const pages = [
      {
        title: '开屏广告页',
        name: 'index',
        path: 'index/index'
      },
      {
        title: '登录页',
        name: 'login',
        path: 'login/index'
      },
      {
        title: '应用首页',
        name: 'home',
        path: 'home/index'
      }
    ];

    module.exports = pages
    ```

- 2 PC 端调试

  - 推荐使用场景

    - 前期开发页面，调试样式时将 debugOnPC 设置为 true，可在 PC 浏览器端调试

    - 后期开发功能，调试功能时将 debugOnPC 设置为 false，在自定义Loader中调试

    - 或者也可以始终将 debugOnPC 设置为 false，在自定义Loader中调试

  - 因为 APICloud 会在 APPLoader 或者 自定义 Loader 中向 `window` 上注入全局变量 `api`, 所以在移动端设备上使用 Loader 调试时可以通过 `api` 来实现调用原生模块的能力。为保证调用原生模块时都已经初始化完成，所有调用原生模块的语句都应该放在 `apiready` 的 `Hooks` 中执行。

  - 当 `debugOnPC` 为默认值即 `false` 时, Vue 页面的实例将在 `apiready` 的 `Hooks` 中执行，因此在 PC 端浏览器内无法预览调试页面。

  - 当 `debugOnPC` 为 true 时，将直接执行 new Vue() 创建页面实例，此时，所有 `api` 相关的语句无法执行，但是与 `api` 无关的页面内容将会正常显示在浏览器内。
  