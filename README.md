# cloudbase-adapter-quickapp

## 介绍

@cloudbase/js-sdk 只支持常规 Web 应用（即浏览器环境）的开发，不兼容其他类 Web 平台，比如微信小程序、快应用、Cocos 等。虽然这些平台大多支持 JavaScript 运行环境，但在网络请求、本地存储、平台标识等特性上与浏览器环境有明显差异。针对这些差异特性，@cloudbase/js-sdk 提供一套完整的适配扩展方案，遵循此方案规范可开发对应平台的适配器，然后搭配 @cloudbase/js-sdk 和适配器实现平台的兼容性。
本项目按照[适配器开发指引](https://docs.cloudbase.net/api-reference/webv3/adapter/)完成Android快应用平台的适配。

## 安装

```
npm install @quickapp-eco/cloudbase-adapter-quickapp -S
```

## 使用

```js
import cloudbase from "@cloudbase/js-sdk"
import adapter, { QappStorage, requestFunction } from '@quickapp-eco/cloudbase-adapter-quickapp'

// 如果是在app.ux里useAdapters，请注意要在onCreate生命周期或者之后调用
// 因为adapter调用到了快应用中的feature
cloudbase.useAdapters(adapter)

// 初始化cloudbase
const app = cloudbase.init({
  env: 'xxx', // 环境ID
  clientId: 'xxx', // 应用ID
  appSign: 'com.example.cloudbase', // 应用标识
  appSecret: {
    appAccessKeyId: 1, // 应用凭证版本号
    appAccessKey: 'xxxx' // 应用凭证
  }
})

// 身份认证，返回 Auth 对象
// auth模块的adapter有问题，需要在构造函数中传参
// 调用与app.auth方法务必按照以下方式传参
const auth = app.auth({
  storage: QappStorage,
  baseRequest: requestFunction,
  captchaOptions: {
    openURIWithCallback: (...props) => {
      console.log('open uri with callback', ...props)
    }
  },
})
```

### Tips:

- 目前应用真正只用到了`system.fetch`，其余的暂时没有应用场景。

- 在manifest.json文件里声明adapter中用到的接口:
    - system.fetch
    - system.request
    - syste.storage
    - system.websocketfactory


## License

[MIT](./LICENSE)
