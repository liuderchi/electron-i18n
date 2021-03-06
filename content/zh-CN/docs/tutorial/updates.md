# 更新应用程序

有多种方法可以更新Electron应用. 最简单并且官方支持的是一个利用内置的[Squirrel](https://github.com/Squirrel)框架和Electron的[autoUpdater](../api/auto-updater.md)模块.

## 部署更新服务器

开始前, 你首先需要部署一个服务器用来自动更新下载[autoUpdater](../api/auto-updater.md)模块.

根据你的需要，你可以从下方选择：

- [Hazel](https://github.com/zeit/hazel)为私有或开源应用程序更新服务器 可以用[Now](https://zeit.co/now)免费部署(使用单个命令), 先从[GitHub Releases](https://help.github.com/articles/creating-releases/)拉取然后利用 GitHub's CDN 的强大能力.
- [Nuts](https://github.com/GitbookIO/nuts)－同样使用[GitHub Releases](https://help.github.com/articles/creating-releases/), 但得在磁盘上缓存应用程序更新并支持私有存储库.
- [electron-release-server](https://github.com/ArekSredzki/electron-release-server) –提供用于处理发布的仪表板
- [Nucleus](https://github.com/atlassian/nucleus) – 一个由Atlassian维护的 Electron 应用程序的完整更新服务器。 支持多种应用程序和渠道; 使用静态文件存储来降低服务器成本.

如果你的app是通过[electron-builder](https://github.com/electron-userland/electron-builder)打包那么你可以使用[electron-updater](https://www.electron.build/auto-update)模块, 它不依赖任何服务器并且可以从S3, GitHub或者任何其它静态文件存储更新.

## 实施更新你的应用程序

一旦你部署了更新服务器, 继续导入你所需要的代码模块. 下列代码可能因不同的服务器软件而变化, 但是它运行时就像[Hazel](https://github.com/zeit/hazel)描述的那样.

**Important:** 请确保下面的代码只在打包的应用程序, 而不是开发中. 你可以使用[electron-is-dev](https://github.com/sindresorhus/electron-is-dev)检查开发环境.

```js
const {app, autoUpdater, dialog} = require('electron')
```

其次, 构建更新服务器的URL然后通知[autoUpdater](../api/auto-updater.md)以下事项:

```js
const server = 'https://your-deployment-url.com'
const feed = `${server}/update/${process.platform}/${app.getVersion()}`

autoUpdater.setFeedURL(feed)
```

最后一步, 检查更新. 下面的例子将每分钟检查一次:

```js
setInterval(() => {
  autoUpdater.checkForUpdates()
}, 60000)
```

一旦你的应用程序被[packaged](../tutorial/application-distribution.md), 它将接收每次你从[GitHub Release](https://help.github.com/articles/creating-releases/)发布的更新.

## 应用更新

现在您已经为应用程序配置了基本的更新机制, 您需要确保在更新时通知用户. 这可以使用autoUpdater API [events](../api/auto-updater.md#events)来实现:

```js
autoUpdater.on('update-downloaded', (event, releaseNotes, releaseName) => {
  const dialogOpts = {
    type: 'info',
    buttons: ['Restart', 'Later'],
    title: 'Application Update',
    message: process.platform === 'win32' ? releaseNotes : releaseName,
    detail: 'A new version has been downloaded. Restart the application to apply the updates.'
  }

  dialog.showMessageBox(dialogOpts, (response) => {
    if (response === 0) autoUpdater.quitAndInstall()
  })
})
```

同时要确保错误被[being handled](../api/auto-updater.md#event-error). 这是一个例子它将记录到`stderr`:

```js
autoUpdater.on('error', message => {
  console.error('There was a problem updating the application')
  console.error(message)
})
```