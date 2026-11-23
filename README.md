# Chimera 安卓端

奇美拉社区的安卓终端：Capacitor 壳 + 论坛网页版本体。

对接论坛 https://forum.coldhao.win 。

> 本应用不另写一套客户端：它直接加载论坛网页版本体（`server.url`），
> 网页版改什么，安卓端就跟着变——不再重蹈桌面端落后网页版的覆辙。
> 论坛本身在另一个仓库；本应用只包一层壳，不 fork 论坛。

## 这是什么

- **完整论坛**：帖子、板块、群聊、好友、私聊、设置、账号绑定——
  网页版有什么，安卓端就有什么（同一套界面、同一套登录流程）。
- **登录**：走网页版本身的登录（用户名 + 密码 / 快捷登录），会话与
  网页版一致；本应用不存密码、不自造登录流程。
- **返回键**：WebView 能后退时先在页面内后退（群聊/帖子/设置之间），
  退到头才退出应用（`MainActivity.java`）。
- **图标与启动画面**：社区品牌图标主版——三动物渐变版（狮子金→橙红、
  山羊银→蓝紫、蛇绿→青，金属银环、水晶星中心，深蓝星空底），
  源文件 `assets/icon-source-three-animals-v4.png`（1600x1600 PNG，
  用户 2026-11-23 21:14 认可），
  sha256 `cc70e5d3d3facbfb4034ed5c09c4820a61b19a37b978e532c2d7b1a27f3f85fb`；
  历史版另存：原子图标 v3 `assets/icon-source-v3.png`，
  sha256 `cde8cb5af32319a06be28823b10f6cf97302f151532fb7bdb38ce1f7c8b6527e`；
  v2 `assets/icon-source-v2.png`，
  sha256 `553534fdb9c5b172ad4ba88e64fc08cff344297bb01fd28d0a721911335c2281`；
  v1 高清版 `assets/icon-source-hd.png`，
  sha256 `7b2040e63f5a9c4fd5043708085eef0baf9308f328fe1e5a59aeb9fe16d33704`；
  256x256 原版 `assets/icon-source.png`，
  sha256 `21e8e6c276c1544b8ff1e298d188addd129560879e6600809d904ce06d28bc59`。
  `assets/icon.png` / `assets/splash.png` 由 `@capacitor/assets` 生成，
  前景/背景走图标的深海军蓝 `#101028`。注意：这张是品牌标记，
  不是可爱风吉祥物——论坛站标与介绍区形象仍用 v4c 吉祥物，两者用途不同。

## 这一版不做的事

- 推送通知（须服务端端点 + 推送服务，另排期）。
- 内置轻节点（桌面端有；安卓端首轮不带）。
- 离线缓存（在线加载网页版本体）。

## 运行与构建

```bash
npm install
npx cap sync android     # 同步 www 与配置到 android/
```

安卓构建须 JDK 21（Capacitor 8 的 `capacitor-android` 编译目标为 Java 21，
JDK 17 会报 `invalid source release: 21`）+ Android SDK（platform 36，
build-tools 35.0.0——AGP 8.13.0 的默认版本，须装；36.0.0 也可同装；
SDK 许可须先接受，见 `android/` 下构建记录）：

```bash
export ANDROID_HOME=$HOME/Android/Sdk
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
cd android && ./gradlew assembleDebug   # 调试 APK：app/build/outputs/apk/debug/
```

发布包（须签名密钥，另行配置）：

```bash
cd android && ./gradlew bundleRelease   # AAB，供 Play Store 上架
```

图标改动后重新生成：

```bash
npx @capacitor/assets generate --android
```

## 许可

MIT，见 [LICENSE](LICENSE)。
