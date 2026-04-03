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






########  查看日志：
```shell
solana logs
```
###### 2.4 安装 VS Code 必装插件
```shell
```
