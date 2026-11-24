# 剪纸堆兼容补丁

为原版剪纸堆（Clip Stack）恢复后台剪贴板记录，并停用旧的日志读取授权方案。重启、首次解锁后，可沿用剪纸堆自己的开机启动和后台监听。

版本：**0.3.0**（versionCode **3**）  
包名：`io.github.wuhua0871.clipstackcompat`

## 支持范围

- Android **12 / API 31 及以上**可安装；没有系统版本白名单或上限。需要 Root 及可正常注入系统框架的 LSPosed，使用传统 Xposed API 82。
- 剪纸堆 **1.9.3 / versionCode 98**，包名 `com.catchingnow.tinyclipboardmanager`，仅主用户 **0**。
- 按系统剪贴板接口是否存在来加载，不再跳过 Android 14、16 等系统。接口需要匹配 `boolean ClipboardService.isDefaultIme(int, String)`，接口缺失或类型变化时跳过系统 Hook 并记入框架日志。
- 已核对 AOSP Android 12、13、14、15、16 的接口；实机成功记录仅有 Android 13、15。其他 ROM 和框架组合尚未验证；工作资料和其他用户不启用适配。Android 10、11 不在本补丁范围内。

## 安装与启用

1. 保留已安装的原版剪纸堆，安装 Release 中的 `ClipStackCompat-0.3.0.apk`。
2. 在 LSPosed 中启用「剪纸堆兼容补丁」。
3. 作用域同时勾选「系统框架」（管理器通常显示为 `system`）和「剪纸堆」（`com.catchingnow.tinyclipboardmanager`）。
4. 重启手机，首次解锁。保留剪纸堆的后台记录、通知及系统允许的开机启动设置。
5. 在其他应用复制几段不同文本，再打开剪纸堆检查是否全部记录。

本模块没有桌面入口或设置页，作者署名也可在框架的模块描述中查看。正常生效后，无须再为旧日志读取方案反复确认授权。

## 从旧自用版迁移

旧包名 `local.clipstack.compat` 与正式版是两个独立模块，不能覆盖升级。先在 LSPosed 停用旧模块，再安装并启用正式版、勾选以上两个作用域，随后重启。确认正常后可卸载旧模块。请勿同时启用两版；请勿卸载原版剪纸堆或清除其数据。

## 工作方式与隐私

模块仅为主用户的剪纸堆增加系统剪贴板后台读取特例，同时关闭原应用的旧 logcat 监听与旧权限引导。沿用原应用的监听、历史、收藏和通知，不修改原版 APK 或历史数据库。

模块自身不申请联网、存储或日志权限，没有后台服务，不收集或上传剪贴板内容。不改变实际默认输入法；系统原有的 UID 校验、AppOps 和锁屏读取限制仍适用。启用后，剪纸堆可在后台读取其他应用复制的内容，包括可能敏感的文本，请按自己的需要启用后台记录。

## 验证与限制

适配逻辑此前已在 Pixel 8 Pro / Android 15（自用版 0.1.0，LSPosed 1.11.0）和 Pixel 5 / Android 13（自用版 0.2.0，LSPosed 1.9.3_mod）完成重启后自动启动与实际后台复制验证；用户确认无须重新打开剪纸堆或再次授权。

0.3.0 保留主用户与包名条件、后台读取和旧日志停用逻辑，去除 Android 13/15 白名单，新增接口存在及返回类型检查，改为正式包名并加入署名。正式 APK 已完成构建、签名、作用域条件、接口匹配和元数据检查，尚未重新安装到手机验证。未完成长时间待机或快速连续复制测试。不保证记录锁屏期间的全部变化，非纯文本支持也沿用原应用能力。接口存在也不代表所有 ROM 的行为完全相同；失效时请停用模块并重启回退。

## 停用与回退

在 LSPosed 停用本模块并重启，即恢复原版行为；也可卸载本模块。回退旧自用版时，先停用正式版，再启用旧版并重启。两者都不会迁移或删除剪纸堆历史。

## 项目与参考

这是独立的第三方兼容补丁，与剪纸堆原作者、Google 或 LSPosed 官方无隶属关系。原版应用请从其原有渠道获取，本项目不分发原版 APK。

- 原版剪纸堆：[Play 商店](https://play.google.com/store/apps/details?id=com.catchingnow.tinyclipboardmanager)、[原作者旧源码](https://github.com/heruoxin/Clip-Stack)。
- 系统接口：[Android 13 ClipboardService](https://android.googlesource.com/platform/frameworks/base/+/refs/tags/android-13.0.0_r1/services/core/java/com/android/server/clipboard/ClipboardService.java)、[Android 15 ClipboardService](https://android.googlesource.com/platform/frameworks/base/+/refs/tags/android-15.0.0_r1/services/core/java/com/android/server/clipboard/ClipboardService.java)。
- 扩展版本核对：[Android 12](https://android.googlesource.com/platform/frameworks/base/+/refs/tags/android-12.0.0_r1/services/core/java/com/android/server/clipboard/ClipboardService.java)、[Android 14](https://android.googlesource.com/platform/frameworks/base/+/refs/tags/android-14.0.0_r1/services/core/java/com/android/server/clipboard/ClipboardService.java)、[Android 16](https://android.googlesource.com/platform/frameworks/base/+/refs/tags/android-16.0.0_r1/services/core/java/com/android/server/clipboard/ClipboardService.java)。
- 相关方案参考：[Clipboard Whitelist](https://github.com/Xposed-Modules-Repo/io.github.tehcneko.clipboardwhitelist)。本模块聚焦剪纸堆，并额外停用其旧日志方案。

## 作者与反馈

作者：**wuhua0871**  
GitHub：[wuhua0871](https://github.com/wuhua0871)  
邮箱：[wuhua0871@live.cn](mailto:wuhua0871@live.cn)  
博客：[wuhua0871.com](https://wuhua0871.com)  
资源站：[app.wuhua0871.com](https://app.wuhua0871.com)

反馈请附手机型号、Android 版本、剪纸堆版本、框架版本和复现步骤；日志中请遮住剪贴板内容等个人信息。
