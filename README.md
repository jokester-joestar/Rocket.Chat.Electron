# Rocket.Chat Desktop App

[![Travis CI Build Status](https://img.shields.io/travis/RocketChat/Rocket.Chat.Electron/master.svg?logo=travis)](https://travis-ci.org/RocketChat/Rocket.Chat.Electron)
[![AppVeyor Build Status](https://img.shields.io/appveyor/ci/RocketChat/rocket-chat-electron/master.svg?logo=appveyor)](https://ci.appveyor.com/project/RocketChat/rocket-chat-electron)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/3a87141c0a4442809d9a2bff455e3102)](https://www.codacy.com/app/tassoevan/Rocket.Chat.Electron?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=RocketChat/Rocket.Chat.Electron&amp;utm_campaign=Badge_Grade)
[![Project Dependencies](https://david-dm.org/RocketChat/Rocket.Chat.Electron.svg)](https://david-dm.org/RocketChat/Rocket.Chat.Electron)
[![GitHub All Releases](https://img.shields.io/github/downloads/RocketChat/Rocket.Chat.Electron/total.svg)](https://github.com/RocketChat/Rocket.Chat.Electron/releases/latest)
![GitHub](https://img.shields.io/github/license/RocketChat/Rocket.Chat.Electron.svg)

可用于 macOS、Windows 和 Linux 的[Rocket.Chat][] 桌面应用程序
使用 [Electron][]。

![Rocket.Chat Desktop App](https://user-images.githubusercontent.com/2263066/91490997-c0bd0c80-e889-11ea-92c7-2cbcc3aabc98.png)

---

## 与我们互动

### 分享你的故事
我们很想听听[您的体验][]，可能会纳入我们的
[博客][].

### 订阅更新
我们的营销团队每月发布一次电子邮件更新，其中包含有关产品新闻的新闻
发布、公司相关主题、事件和用例。[报名!][]

---

## 下载

你可以在[发布][] 页面获取到最新的版本。

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-black.svg)](https://snapcraft.io/rocketchat-desktop)

## 安装

启动安装程序并按照说明进行安装。

### Windows 选项

在 Windows 上，您可以通过添加`/S`标志来运行静默安装。也可以
添加以下选项：

- `/S` - 静默安装
- `/allusers` - 为所有用户安装（需要管理员）
- `/currentuser` - 仅为当前用户安装 （默认）
- `/disableAutoUpdates` - 禁用自动更新

## 开发相关

### 快速入门

前提准备:

- [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Node.js](https://nodejs.org)
- [node-gyp](https://github.com/nodejs/node-gyp#installation)
- [Yarn](http://yarnpkg.com/) 建议使用这个代替npm。

现在只需克隆并启动应用程序：

```sh
git clone https://github.com/RocketChat/Rocket.Chat.Electron.git
cd Rocket.Chat.Electron
yarn
yarn start
```

### 项目结构

源代码存放在`src`文件夹中。
在使用`yarn start`运行应用程序后，这些内容会自动构建。

构建过程和编译的所需要的支持来自`src`文件夹及其包含的
`app`文件夹，因此在构建完成后，您的`app`文件夹将包含
完整、可运行的应用程序。

### TypeScript

在 [Rocket.Chat核心代码一顿狂改][]之后，又使用 TypeScript 4 重写了一遍，用来解决可维护性的问题。

### 构建管线

构建过程基于 [rollup][] 捆绑器。有三个入口文件
对于您的代码：

- `src/main.ts`，在Electron的主进程上运行的脚本，编排
  整个应用程序;

- `src/rootWindow.ts`，呈现 *root window* 的 UI 的脚本，
  应用程序的主窗口;

- 和`src/preload.ts`，它以特权模式运行以连接应用程序和
  呈现 Rocket.Chat 的网络客户端的网络视图。

#### 添加 Node.js 模块

请遵守在`package.json`文件中
对`dependencies`和`devDependencies`之间的拆分。
只有`dependencies`中列出的模块才会包含在可分配的应用中。

### 故障排除

#### node-gyp

按照上面的安装说明进行操作 [node-gyp readme][].

#### Ubuntu

您可能需要安装以下软件包：

```sh
build-essential
libevas-dev
libxss-dev
```

#### Fedora

您可能需要安装以下软件包：

```sh
libX11
libXScrnSaver-devel
gcc-c++
```

#### Windows 7

在 Windows 7 上，您可能需要遵循 [node-gyp install guide][] 的选项 2
并安装 Visual Studio。

### 测试

#### 单元测试

```sh
yarn test
```

我们使用[Jest][]测试框架和[Jest electron runner][]。它搜索
对于`src`目录中与 glob 模式匹配的所有文件
`*.(spec|test).{js,ts,tsx}`.

### 打包发布

要将应用打包到安装程序中，请使用以下命令：

```sh
yarn release
```

它将启动您正在运行的操作系统的打包过程
命令打开。准备分发文件将被输出到`dist`目录。

所有打包操作都由 [electron-builder][] 处理。它有很多
[自定义选项][].

## 默认服务器

客户端连接到哪个服务器由`servers.json`文件决定，并且在侧边栏中显示服务器列表。

它包含一个默认列表
将在用户首次运行应用时添加的服务器（或当所有服务器将从列表中删除）。
文件语法如下：

```json
{
  "Demo Rocket Chat": "https://demo.rocket.chat",
  "Open Rocket Chat": "https://open.rocket.chat"
}
```

### 预发布配置

您可以将`servers.json`与安装包捆绑在一起，该文件应位于项目应用程序的根目录中（与`package.json`级别相同）。
 如果找到该文件，将跳过初始的“连接到服务器”屏幕，它将尝试连接到已定义的阵列中的第一台服务器，并将用户直接放在登录屏幕上。
请注意，只有在尚未添加其他服务器的情况下，才会检查`servers.json`，
当你卸载了应用程序但没有删除旧的首选项时，`servers.json`不会被触发。

### 安装后配置

如果您不能（或不想）将文件捆绑在应用程序中，则可以在用户首选项文件夹中创建一个`servers.json`，该文件夹将覆盖打包的文件。该文件应位于`%APPDATA%/Rocket.Chat/`文件夹中，或者在为所有用户安装的情况下安装文件夹（仅限Windows）。

对于 Windows，完整路径为：

- `~\Users\<username>\AppData\Roaming\Rocket.Chat\`
- `~\Program Files\Rocket.Chat\Resources\`

对于 macOS，完整路径为：

- `~/Users/<username>/Library/Application Support/Rocket.Chat/`
- `/Library/Preferences/Rocket.Chat/`

对于 Linux，完整路径为：

- `/home/<username>/.config/Rocket.Chat/`
- `/opt/Rocket.Chat/resources/`

### 被覆盖的设置

您可以通过在用户首选项文件夹中创建`overridden-settings.json`来覆盖用户设置。
该文件应位于`%APPDATA%/Rocket.Chat/`文件夹中，或者在为所有用户安装的情况下安装文件夹（仅限Windows）。

在文件上设置的每个设置都将覆盖默认设置和用户设置。然后，您可以使用它来禁用自动更新等默认功能，甚至可以创建单个服务器模式。

#### 可以覆盖的设置包括：

| 设置    | 描述 |
| ----------- | ----------- |
| `"isReportEnabled": true,`                   | 设置是否将错误报告给开发人员。 |
| `"isInternalVideoChatWindowEnabled": true,`  | 设置视频通话在内部窗口中打开。 |
| `"isFlashFrameEnabled": true,`               | 设置是否启用闪光帧。 |
| `"isMinimizeOnCloseEnabled": false,`         | 设置应用是否在关闭时最小化。 |
|`"doCheckForUpdatesOnStartup": true,`         | 设置应用是否将在启动时检查更新。 |
| `"isMenuBarEnabled": true,`                  | 设置是否启用菜单栏。 |
|`"isTrayIconEnabled": true,`                  | 启用托盘图标，应用程序将在关闭时隐藏到托盘中。覆盖 `"isMinimizeOnCloseEnabled"` |
|`"isUpdatingEnabled": true,`                  | 设置用户是否可以更新应用。 |
|`"isAddNewServersEnabled": true,`              | 设置用户是否可以添加新服务器。 |

##### 单服务器模式
如果设置了`"isAddNewServersEnabled": false`，则用户将无法添加新服务器。
按钮和快捷方式将被禁用。然后，您必须将服务器添加到`servers.json`文件中。
有了这个，您可以创建单个服务器模式，或者只是不让用户自己添加新服务器。

##### 配置示例
`overridden-settings.json` 文件:

    {
       "isTrayIconEnabled": false,
       "isMinimizeOnCloseEnabled": false
    }
启用`isTrayIconEnabled`时，该应用程序将在关闭时隐藏。
启用`isMinimizeOnCloseEnabled`时，应用程序将在关闭时最小化。
当两者都被禁用时，应用程序将在关闭时退出。

## 许可证

在 MIT 许可下发布。

[Rocket.Chat]: https://rocket.chat

[Electron]: https://electronjs.org/

[您的体验]: https://survey.zohopublic.com/zs/e4BUFG

[博客]: https://rocket.chat/case-studies/?utm_source=github&utm_medium=readme&utm_campaign=community

[报名!]: https://rocket.chat/newsletter/?utm_source=github&utm_medium=readme&utm_campaign=community

[发布]: https://github.com/RocketChat/Rocket.Chat.Electron/releases/latest

[Rocket.Chat核心代码一顿狂改]: https://forums.rocket.chat/t/moving-away-from-meteor-and-beyond/3270

[rollup]: https://github.com/rollup/rollup

[node-gyp readme]: https://github.com/nodejs/node-gyp#installation

[Jest]: https://jestjs.io/

[Jest electron runner]: https://github.com/facebook-atom/jest-electron-runner

[electron-builder]: https://github.com/electron-userland/electron-builder

[自定义选项]: https://github.com/electron-userland/electron-builder/wiki/Options

[node-gyp install guide]: https://github.com/nodejs/node-gyp#installation
