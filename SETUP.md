# Skills Manager 启动前置指南

## 1. 环境要求

| 依赖       | 版本要求   |
| ---------- | ---------- |
| Node.js    | 18+        |
| npm        | 9+         |
| Rust/Cargo | 最新稳定版 |

## 2. 安装步骤

### 2.1 安装 Node.js

访问 [nodejs.org](https://nodejs.org/) 下载并安装 LTS 版本。

验证安装：

```bash
node --version
npm --version
```

### 2.2 安装 Rust

**Windows 用户**可通过以下任一方式安装 Rust：

---

#### 方案 A：PowerShell 命令安装（推荐）

适合习惯命令行的用户，一键完成下载和安装。

1. **以管理员身份打开 PowerShell**（在搜索栏输入 `PowerShell`，右键选择"以管理员身份运行"）

2. **执行以下命令下载并运行安装程序**：

   ```powershell
   # 下载 rustup-init.exe
   Invoke-WebRequest -Uri https://win.rustup.rs -OutFile rustup-init.exe

   # 运行安装程序（自动选择默认选项）
   .\rustup-init.exe -y
   ```

3. **等待安装完成**，看到 `Rust is installed now. Great!` 即表示成功

4. **按任意键关闭窗口**

---

#### 方案 B：手动下载安装（图形界面）

适合偏好图形界面的用户，手动控制安装过程。

##### 步骤 1：下载安装程序

1. 打开浏览器，访问 https://rustup.rs/
2. 在页面中找到 **"rustup-init.exe"** 下载链接（通常是页面上的第一个大按钮）
3. 点击下载，将 `rustup-init.exe` 保存到电脑

##### 步骤 2：运行安装程序

1. 找到下载的 `rustup-init.exe` 文件（通常在 `下载` 文件夹）
2. **双击运行** `rustup-init.exe`
3. 会弹出一个命令行窗口，显示安装选项：

   ```
   Welcome to Rust!

   This will download and install the official compiler for the Rust
   programming language, and its package manager, Cargo.

   1) Proceed with standard installation (default)
   2) Customize installation
   3) Cancel installation
   >
   ```

4. 输入 `1`（选择标准安装），然后按 **回车**

##### 步骤 3：等待安装完成

- 安装程序会自动下载并安装 Rust 编译器 (`rustc`)、Cargo 包管理器和其他必要工具
- 这个过程可能需要几分钟，取决于你的网络速度
- 安装完成后会显示：

  ```
  Rust is installed now. Great!

  To get started you may need to restart your current shell.
  This would reload its PATH environment variable to include
  Cargo's bin directory (%USERPROFILE%\.cargo\bin).

  Press any key to continue...
  ```

5. 按任意键关闭窗口

---

#### 步骤 4：重启终端/IDE（关键步骤）

**这一步非常重要！**

1. 按 `Win + R`，输入 `sysdm.cpl` 回车
2. 点击 **高级** → **环境变量**
3. 在 **用户变量** 中找到 `Path`，点击 **编辑**
4. 添加条目：`%USERPROFILE%\.cargo\bin`
5. 保存并重启 IDE

Rust 安装程序修改了系统环境变量，但**当前运行的终端/IDE 还没有加载新的环境变量**。

必须执行以下操作之一：

- **方案 A（推荐）**：完全关闭并重新打开 Trae IDE
- **方案 B**：关闭当前终端标签页，新建一个终端
- **方案 C**：注销并重新登录 Windows 账户

#### 步骤 5：验证安装

重启 Trae IDE 后，打开终端并运行以下命令验证 Rust 是否安装成功：

```bash
rustc --version
cargo --version
```

如果显示类似以下的版本号，说明安装成功：

```
rustc 1.78.0 (9b00956e5 2024-04-29)
cargo 1.78.0 (54d8815d0 2024-03-26)
```

如果仍然提示 `command not found` 或 `无法将"cargo"项识别为 cmdlet`，说明环境变量没有正确加载，请返回 **步骤 4** 重启 IDE。

#### 常见问题：cargo 命令未找到

如果在 VS Code/Trae 终端中提示 `cargo : 无法将"cargo"项识别为 cmdlet`，说明环境变量未正确加载：

**解决方案 1**：完全关闭并重新打开 IDE。

**解决方案 2**：临时添加 PATH（当前会话有效）：

```powershell
$env:PATH = "$env:PATH;$env:USERPROFILE\.cargo\bin"
```

**解决方案 3**：永久添加环境变量到系统变量（推荐）：

> **注意**：如果在当前用户命令行中可以执行 `cargo --version`，但在项目终端中执行失败（提示 `command not found` 或 `无法将"cargo"项识别为 cmdlet`），说明环境变量未正确传递到项目环境。此时需要在**系统变量**的 `Path` 中添加同样的条目 `%USERPROFILE%\.cargo\bin`，然后保存并重启 IDE。

1. 按 `Win + R`，输入 `sysdm.cpl` 回车
2. 点击 **高级** → **环境变量**
3. 在 **系统变量**（注意不是用户变量）中找到 `Path`，点击 **编辑**
4. 添加条目：`%USERPROFILE%\.cargo\bin`
5. 保存并重启 IDE

### 2.3 安装项目依赖

```bash
npm install
```

## 3. 启动开发服务器

```bash
npm run tauri:dev
```

## 4. 常用命令

| 命令                  | 说明             |
| --------------------- | ---------------- |
| `npm run tauri:build` | 构建生产版本     |
| `npm run cli:build`   | 构建 CLI 工具    |
| `npm run cli:install` | 安装 CLI 到 PATH |

## 5. 技术栈

| 层级   | 技术                                     |
| ------ | ---------------------------------------- |
| 前端   | React 19, TypeScript, Vite, Tailwind CSS |
| 桌面端 | Tauri 2                                  |
| 后端   | Rust                                     |
| 存储   | SQLite                                   |
