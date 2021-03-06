# 崩溃日志报告

> 将崩溃日志提交给远程服务器

进程： [Main](../glossary.md#main-process), [renderer](../glossary.md#renderer-process) 进程

以下是一个自动提交崩溃日志到服务器的示例

```javascript
const {crashReporter} = require('electron')

crashReporter.start({
  productName: 'YourName',
  companyName: 'YourCompany',
  submitURL: 'https://your-domain.com/url-to-submit',
  uploadToServer: true
})
```

构建一个用于接受和处理崩溃日志的服务，你需要以下工程

* [socorro](https://github.com/mozilla/socorro)
* [mini-breakpad-server](https://github.com/electron/mini-breakpad-server)

崩溃日志被保存在本地的应用特有的临时文件夹中。 对于`YourName`对象中的`productName`，崩溃报日志将被储存在临时文件夹中名为`YourName Crashes`的文件文件夹中。 在启动崩溃报告器之前，您可以通过调用`app.setPath('temp', 'my/custom/temp')` API来自定义这些临时文件的保存路径

## 方法

` crashReporter ` 模块具有以下方法:

### `crashReporter.start(options)`

* `options` Object 
  * `companyName` 字符串 - 公司名称 (可选).
  * `submitURL` 字符串 - 崩溃日志将以POST的方式发送给此URL.
  * `productName` 字符串(可选) - 默认为 `app.getName()`.
  * `uploadToServer` 布尔型(可选) - 控制是否将崩溃日志发送给服务器，默认为`true`.
  * `ignoreSystemCrashHandler` 布尔型(可选) - 默认为`false`.
  * `extra` 对象(可选) - 一个随崩溃日志发送的对象. 只有字符串属性能够被正确发送. 不支持发送嵌套对象，且属性名称和属性值必须小于64个字符长度.

你需要调用任何其他的`crashReporter` API，您必须调用此方法. 在每个需要收集崩溃日志的进程 (主进程 / 渲染器进程) 中，也必须先调用此方法. 从不同的进程调用时, 可以传不同的配置给 ` crashReporter. start `。

** 注意 **由 ` child_process ` 模块创建的子进程将无法访问 Electron 模块。 因此, 要收集它们的故障报告, 请用 ` process.crashReporter.start `代替。 传递与上面相同的选项以及一个名为 ` crashesDirectory ` 的附加项, 它应指向一个目录, 以便临时存储崩溃报告。 你可以调用 ` process.crash()` 使子进程崩溃，来测试结果。

** 注意: **要从 Windows 中的子进程收集崩溃报告, 您也需要添加这样额外的代码。 这将启动一个进程来监视和发送崩溃报告。 用适当的值去替换 ` submitURL `、` productName ` 和 ` crashesDirectory `。

** 注意: **如果您需要在第一次调用 ` start ` 后发送 附加的/更新的 ` extra ` 参数, 在 macOS 上，你可以调用 ` setExtraParameter `, 或在 Linux 和 Windows 上使用 新的/更新的 ` extra ` 参数并再次调用 `start `。

```js
 const args = [
   `--reporter-url=${submitURL}`,
   `--application-name=${productName}`,
   `--crashes-directory=${crashesDirectory}`
 ]
 const env = {
   ELECTRON_INTERNAL_CRASH_SERVICE: 1
 }
 spawn(process.execPath, args, {
   env: env,
   detached: true
 })
```

**Note:** On macOS, Electron uses a new `crashpad` client for crash collection and reporting. If you want to enable crash reporting, initializing `crashpad` from the main process using `crashReporter.start` is required regardless of which process you want to collect crashes from. Once initialized this way, the crashpad handler collects crashes from all processes. You still have to call `crashReporter.start` from the renderer or child process, otherwise crashes from them will get reported without `companyName`, `productName` or any of the `extra` information.

### `crashReporter.getLastCrashReport()`

Returns [`CrashReport`](structures/crash-report.md):

Returns the date and ID of the last crash report. If no crash reports have been sent or the crash reporter has not been started, `null` is returned.

### `crashReporter.getUploadedReports()`

Returns [`CrashReport[]`](structures/crash-report.md):

Returns all uploaded crash reports. Each report contains the date and uploaded ID.

### `crashReporter.getUploadToServer()` *Linux* *macOS*

Returns `Boolean` - Whether reports should be submitted to the server. Set through the `start` method or `setUploadToServer`.

**Note:** This API can only be called from the main process.

### `crashReporter.setUploadToServer(uploadToServer)` *Linux* *macOS*

* `uploadToServer` Boolean *macOS* - Whether reports should be submitted to the server

This would normally be controlled by user preferences. This has no effect if called before `start` is called.

**Note:** This API can only be called from the main process.

### `crashReporter.setExtraParameter(key, value)` *macOS*

* `key` String - Parameter key, must be less than 64 characters long.
* `value` String - Parameter value, must be less than 64 characters long. Specifying `null` or `undefined` will remove the key from the extra parameters.

Set an extra parameter to be sent with the crash report. The values specified here will be sent in addition to any values set via the `extra` option when `start` was called. This API is only available on macOS, if you need to add/update extra parameters on Linux and Windows after your first call to `start` you can call `start` again with the updated `extra` options.

## Crash Report Payload

The crash reporter will send the following data to the `submitURL` as a `multipart/form-data` `POST`:

* `ver` String - The version of Electron.
* `platform` String - e.g. 'win32'.
* `process_type` String - e.g. 'renderer'.
* `guid` String - e.g. '5e1286fc-da97-479e-918b-6bfb0c3d1c72'
* `_version` String - The version in `package.json`.
* `_productName` String - The product name in the `crashReporter` `options` object.
* `prod` String - Name of the underlying product. In this case Electron.
* `_companyName` String - The company name in the `crashReporter` `options` object.
* `upload_file_minidump` File - The crash report in the format of `minidump`.
* All level one properties of the `extra` object in the `crashReporter` `options` object.