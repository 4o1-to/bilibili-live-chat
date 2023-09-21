# Bilibili Live Chat

![Preview](https://i.loli.net/2020/06/20/vXuZKCq396co2HO.gif)

这是一个无后端的，仿 YouTube Live Chat 的，箱都不用开就能食用的 Bilibili 直播弹幕姬

主要用于 OBS，为的是在低功能需求的情况下，不依靠任何第三方本地软件实现弹幕和礼物的展示

老版本在 `v1` 分支，新版本是使用 Vue 3 重构的版本，并增加了一些新特性，成品直接部署在 Github Pages

## 食用步骤

1. 打开 [blc.lolicon.app](https://blc.lolicon.app/)
2. 输入房间号，填写设置项，点击“Go!”，然后复制新页面的地址
3. 在 OBS 中添加“浏览器”来源，将地址粘贴到“URL”处，根据自己需要调整宽高和缩放
4. Enjoy~

## 其他说明

### “获取房间失败”错误

由于B站 API 无法跨域调用，因此获取房间信息需要通过 [codetabs](https://codetabs.com/cors-proxy/cors-proxy.html) 来绕开跨域限制，但这并不是一个可靠的方案，会受该服务的网络状况、稳定性、B站是否拉黑等因素影响

如果出现“获取房间失败”错误，请手动填写“主播UID”，并且如果直播间号填写的是短号，请手动获取长号（可以进到UP主空间查看右侧直播间链接末尾数字）

如果你在 OBS 中使用，建议使用“直接跨域”功能，这样无需依赖 codetabs，具体说明见下文

### 显示头像

> 已支持从弹幕信息中获取头像，不再需要调用 API

头像加载机制：

- 获取到头像后，图片会被预加载，加载完毕或超时（5 秒）后弹幕才会被插入弹幕列表
- 非 GIF 头像会优先加载小头像（48x48）以节省流量，若首包到达时间超过 2 秒（B站 COS 图片压缩处理卡了，偶尔可能发生），则会回退为加载完整大小的头像图片

### 直接跨域

对于上面所述的无法跨域调用B站 API 的问题有一个解决方法：

任何基于 Chromium 的浏览器（例如 OBS Browser 和 Chrome）都可以通过添加 `--disable-web-security` 启动参数来禁用网页安全机制，此时可以开启“直接跨域”选项，所有B站 API 将被直接跨域调用，这样就不需要依赖任何第三方 API

示例：

- OBS：直接在启动的快捷方式后追加该参数，然后通过快捷方式启动即可  
  ![obs](https://i.loli.net/2020/06/20/QkXOfoTalnpAvt3.png)
- Chrome：和 OBS 同理，不过必须额外添加一个  `--user-data-dir` 参数来指定用户目录，随意新建一个空文件夹来指定即可  
  该操作看上去十分麻烦，实则是 Chrome 的一个安全措施，因为**禁用网页安全机制是危险行为，日常使用时千万别这么做**  
  ![chrome](https://i.loli.net/2020/06/20/BRvQ2HyjgFeEP73.png)

其他内核的浏览器可以自行搜索相应参数来禁用网页安全机制

## Project setup

```bash
yarn install
```

### Compiles and hot-reloads for development

```bash
yarn serve
```

### Compiles and minifies for production

```bash
yarn build
```

### Lints and fixes files

```bash
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
