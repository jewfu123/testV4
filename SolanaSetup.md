### Solana Setup in the new Ubuntu 24.04 computer.
#### About tools commands
###### 1. Install node.js and nvm version mananger
```shell
nvm install 20
nvm use 20
node -v # 确认版本
```
###### 2.1 Install Hardhat (行业标准，基于 JS/TS)
```shell
npm install --save-dev hardhat
```
###### 2.2 Install Foundry (极致速度，基于 Rust)
```shell
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup
```
###### 2.3 安装 VS Code 必装插件
```shell
打开 VS Code (code .)，在插件市场（Extensions）搜索并安装以下插件：

Solidity (by Juan Blanco)：

用途：提供 Solidity 语法的智能提示、高亮和代码格式化。

Hardhat for Visual Studio Code：

用途：集成 Hardhat 命令，直接在编辑器里调试智能合约。

Truffle for VS Code：

用途：即使你不用 Truffle 框架，它提供的“区块链网络管理”功能也非常直观。

Prettier - Code installer：

用途：保持代码整洁。
```
###### 2.4 安装基础加密工具 (Rust & Go) 安装 Rust (区块链开发的“第二语言”)：
```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```
###### 2.5 安装 GO
```shell
sudo apt install golang-go
```
###### 2.6 本地私链测试环境：Ganache
```shell
npm install -g ganache
```
#### 针对你 AX15 笔记本的优化建议
###### (1) 内存管理：区块链编译（尤其是使用 Hardhat）比较吃 CPU 和内存。既然你有 16GB，建议在 hardhat.config.js 中适当调高优化器次数（optimizer runs），以换取更小的部署体积。
###### (2) Docker 协同：如果你要研究 Hyperledger Fabric 或者搭建本地以太坊全节点，记得利用你之前装的 Docker：
```shell
# 快速启动一个本地以太坊节点镜像
docker run -it -p 8545:8545 ethereum/client-go:latest --dev --http --http.addr 0.0.0.0
```
###### 2.7 安装 VS Code 必装插件
```shell
Solidity (by Juan Blanco)：

用途：提供 Solidity 语法的智能提示、高亮和代码格式化。

Hardhat for Visual Studio Code：

用途：集成 Hardhat 命令，直接在编辑器里调试智能合约。

Truffle for VS Code：

用途：即使你不用 Truffle 框架，它提供的“区块链网络管理”功能也非常直观。

Prettier - Code installer：

用途：保持代码整洁。
```
###### 2.8 由于 Solana 的工具链比较特殊，我们需要先安装它的专属 CLI
```shell
1. 安装 Solana 工具链
sh -c "$(curl -sSfL https://release.solana.com/v1.18.4/install)"
*安装完成后，根据提示将路径加入 .bashrc（通常是 export PATH="/home/jewfu/.local/share/solana/install/active_release/bin:$PATH"），然后执行 source ~/.bashrc。
2. 创建一个基础 Anchor 项目
  (1) 安装 Anchor CLI:
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm install latest
avm use latest
  (2) 初始化项目:
anchor init my_solana_dapp
cd my_solana_dapp
  (3) 基础 Rust 合约代码 (Programs)
  (4) 编译、运行与部署（本地环境）
     1) 生成本地钱包密钥对（如果你还没有）：
solana-keygen new --no-bip39-passphrase
     2) 设置配置为本地网络:
solana config set --url localhost
     3) 启动本地测试节点（重要： 请新开一个终端运行，不要关闭）：
solana-test-validator
     4) 编译项目:
anchor build
     5) 部署到本地节点:
anchor deploy
```

###### 2.9 给你的“职人”调试技巧
######  1) 查看日志：
```shell
solana logs
```
######  2) VS Code 插件增强：
```shell
搜索并安装 "anchor-analyzer"，它能针对 Anchor 框架提供更精准的 Rust 语法检查。
```
######  3) 内存占用：
```shell
运行 solana-test-validator 时会占用约 1GB-2GB 内存。16GB，建议同时开着 System Monitor 观察一下，确保它不会和你之前开启的 XXXXXX 抢夺资源。
```

--------------------------------------
这个错误 SSL_ERROR_SYSCALL 通常意味着连接在握手阶段被强行重置了。

########  方案一：修复证书与重试（最快）有时候是因为 ca-certificates 没有更新。
```shell
sudo apt update && sudo apt install -y ca-certificates
# 尝试使用 -k 忽略证书校验（临时方案，仅用于确认是否为证书问题）
sh -c "$(curl -sSfL -k https://release.solana.com/v1.18.4/install)"
```
########  方案二：利用代理（如果你有配置好的工具） 如果你已经在 Ubuntu 上配置了类似 Xray 或系统代理，你需要告诉 curl 走代理端口：
```shell
# 假设你的代理端口是 1080
export https_proxy=http://127.0.0.1:1080
sh -c "$(curl -sSfL https://release.solana.com/v1.18.4/install)"
```
########  方案三：手动下载安装（100% 成功）
```shell
如果 curl 脚本一直断线，我们可以手动把那个几百 MB 的预编译包抓下来。

直接下载压缩包：
在浏览器中打开：https://github.com/solana-labs/solana/releases/download/v1.18.4/solana-release-x86_64-unknown-linux-gnu.tar.bz2
（如果浏览器能下载，说明只是 curl 的握手问题）

解压并移动：
cd ~/Downloads
tar jxf solana-release-x86_64-unknown-linux-gnu.tar.bz2
cd solana-release/
# 将 bin 目录移动到你的家目录下
mkdir -p ~/.local/share/solana/install/active_release
cp -r bin ~/.local/share/solana/install/active_release/

配置 PATH：
打开 ~/.bashrc，在末尾加上：
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"

执行 source ~/.bashrc。
```

###### 2.9 清理残留的代理环境变量
```shell
unset http_proxy
unset https_proxy
unset ALL_PROXY
```


###### 2.10 强制同步 Solana 编译器组件 anchor build 依赖一个名为 platform-tools 的组件。如果它下载不完整，就会报错。请执行以下命令手动触发下载：
```shell
# 1. 检查 solana 版本
solana --version

# 2. 运行组件安装/同步命令
solana-install init 1.18.4

# 1. 强制系统下载并安装编译器工具链
solana-install init 1.18.4

# 2. 验证底层编译器是否能运行
cargo-build-sbf --version

如果提示 solana-install: command not found，说明你之前的 PATH 没设对。请执行 source ~/.bashrc 确保 ~/.local/share/solana/install/active_release/bin 已经生效。
```

###### 2.11 手动安装 BPF SDK (最有效的方案)
```shell
如果 anchor build 还是报错，我们需要手动“拉”一把编译器：

# 直接运行这个命令来触发编译器的内部下载和配置
cargo-build-sbf --version

如果这个命令也报错 NotFound，请确认你的 Solana PATH 路径。通常编译器位于：
~/.local/share/solana/install/active_release/bin/

*补齐最基础的解压工具
sudo apt update
sudo apt install -y bzip2 tar gzip ca-certificates

*手动触发 Platform Tools 下载
# 强制系统去下载编译器后端
solana-keygen new --no-bip39-passphrase # 确保你有身份文件
cargo-build-sbf --version

# 删除可能损坏的旧工具链
rm -rf ~/.cache/solana/v1.18.4

# 再次尝试编译（它会自动重新下载 platform-tools）
anchor build

*检查隐藏的路径依赖 (关键)
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
```

###### 2.12 彻底切断旧版干扰
```shell
which solana
which cargo-build-sbf
```

###### 2.13 强制升级工具链（最有效的一步）既然 v1.39 太老，我们直接强制让 Solana 升级到目前最稳定的 v1.18 系列，并自动匹配对应的编译器：
```shell
# 强制覆盖安装最新稳定版
solana-install init 1.18.12
```

###### 2.14 核心修复方案：强制更新 Rust 与 Solana 工具链
```shell
  1. 更新你的全局 Rust 版本
rustup update stable
  2. 删除冲突的 Cargo.lock
cd ~/Documents/code/solana/my_solana_dapp
rm Cargo.lock
  3. 强制 Solana 使用最新的编译插件
# 再次运行这个，确保它重新下载并链接正确的组件
solana-install init 1.18.12
  4. 终极编译命令（带上兼容参数）
# 1. 尝试直接用 cargo 编译程序部分
cargo build-sbf

# 2. 如果还报 lock file 错误，尝试带上这个参数（针对 Rust 新特性的临时方案）
cargo build-sbf -- --locked
```

###### 2.15 手动安装方案 (100% 成功) 既然 anchor build 写不进那个 .tar.bz2 文件，我们直接帮它把文件放进去。
```shell
1. 手动下载编译器包
请在浏览器中直接打开以下链接（或者在终端用你配置好的代理下载）：
https://github.com/solana-labs/platform-tools/releases/download/v1.52/platform-tools-linux-x86_64.tar.bz2

2. 创建目录并解压
下载完成后（假设文件在 ~/Downloads），执行以下命令：
# 1. 创建目标版本目录（报错信息显示它在找 v1.52）
mkdir -p ~/.cache/solana/v1.52

# 2. 将压缩包解压到该目录下
# 注意：解压后目录结构应该是 ~/.cache/solana/v1.52/platform-tools/...
tar -jxf ~/Downloads/platform-tools-linux-x86_64.tar.bz2 -C ~/.cache/solana/v1.52/

3. 验证目录结构
确保你的目录长这样（非常重要）：
ls ~/.cache/solana/v1.52/platform-tools/
# 你应该能看到 bin, llvm, rust 等文件夹

重新触发编译
现在底层工具已经手动“喂”进去了，再次回到项目目录运行：
cd ~/Documents/code/solana/my_solana_dapp
anchor build
```


###### 2.16 离线手动修复方案 (100% 成功) 既然 anchor build 告诉你它缺东西，咱们就手动把东西塞进它的“嘴”里。
```shell
1. 手动下载所需的二进制包
请在浏览器中（或者开启你的 Xray 代理后用浏览器）直接下载这个文件：

下载地址：platform-tools-linux-x86_64.tar.bz2 (v1.41)
注：如果系统以后提示需要 v1.52，方法也是一样的，只是版本号改一下。

2. 手动创建目录并解压
假设你已经下载到了 ~/Downloads，请执行：

# 1. 清理刚才中断产生的残留坏文件
rm -rf ~/.cache/solana/v1.41

# 2. 创建正确的版本目录
mkdir -p ~/.cache/solana/v1.41

# 3. 解压到该目录
tar -jxf ~/Downloads/platform-tools-linux-x86_64.tar.bz2 -C ~/.cache/solana/v1.41/

3. 强制修复“Toolchain Not Installed”报错
因为你中断了安装，Solana 的内部 Rust 链接断了。执行以下命令重新初始化工具链：

# 重新安装并链接组件（因为文件已经手动放进去了，这一步会非常快）
solana-install init 1.18.4

最终尝试编译
重新打开一个新的终端窗口（确保环境变量刷新），然后执行：

cd ~/Documents/code/solana/my_solana_dapp
anchor build


```

