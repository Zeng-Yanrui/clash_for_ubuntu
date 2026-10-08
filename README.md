# Clash Party Linux 安装包（Ubuntu 20.04 amd64 / arm64）

**适用于 Ubuntu 20.04 amd64/arm64**

安装包采用 20MB 分卷压缩（zip 分卷），请根据设备架构选择对应的一组分卷，全部下载后按下面的步骤解压、校验、安装即可。

## 文件列表

每个架构的安装包分别放在同名目录中，目录内包含分卷和校验文件：

```text
clash-party-linux-2.0.3-amd64.deb/                # Ubuntu 20.04（x86_64）
├── clash-party-linux-2.0.3-amd64.deb.z01 ~ z09   分卷 1~9（每个 20MB）
├── clash-party-linux-2.0.3-amd64.deb.zip         分卷 10（最后一个，约 13MB）
└── clash-party-linux-2.0.3-amd64.deb.sha256      完整安装包的 SHA256 校验值

clash-party-linux-2.0.3-arm64.deb/                # Jetson NX（ARM64）
├── clash-party-linux-2.0.3-arm64.deb.z01 ~ z08   分卷 1~8（每个 20MB）
├── clash-party-linux-2.0.3-arm64.deb.zip         分卷 9（最后一个，约 20MB）
└── clash-party-linux-2.0.3-arm64.deb.sha256      完整安装包的 SHA256 校验值
```

> 解压前请确认该架构的所有分卷（`.z01` ~ `.zip`）都已下载完整。

## 1. 解压

进入对应架构的目录，把所有分卷按顺序拼接后解压（后续步骤都在该目录中执行）：

```bash
# amd64
cd clash-party-linux-2.0.3-amd64.deb
cat clash-party-linux-2.0.3-amd64.deb.z0* clash-party-linux-2.0.3-amd64.deb.zip > /tmp/clash-party.zip
unzip /tmp/clash-party.zip

# arm64
cd clash-party-linux-2.0.3-arm64.deb
cat clash-party-linux-2.0.3-arm64.deb.z0* clash-party-linux-2.0.3-arm64.deb.zip > /tmp/clash-party.zip
unzip /tmp/clash-party.zip
```

> `unzip` 处理分卷时会输出几条 `bad zipfile offset` 之类的警告，属正常现象，不影响解压结果；不确定时可按下节校验哈希。系统若未安装 `unzip`，执行 `sudo apt install unzip`。

Windows：用 7-Zip / WinRAR / Bandizip 打开 `.zip` 结尾的文件解压即可。

## 2. 校验完整性(可选)

计算解压出的安装包哈希值：

```bash
sha256sum clash-party-linux-2.0.3-amd64.deb    # 或 arm64 对应文件
```

将输出与同名 `.sha256` 文件中的值对比，应完全一致：

| 架构 | SHA256 |
| --- | --- |
| amd64 | `a45ecdecba85b6f3662851a6013d31874ac5e0f91df855dc68b39aec4b2c463d` |
| arm64 | `9d7d4bb9fb168f4dc5878df0c79ebd949190c52e191711d90fbc1a610af797d9` |

也可以一条命令自动校验（把文件名换成自己的架构即可）：

```bash
echo "$(cat clash-party-linux-2.0.3-amd64.deb.sha256)  clash-party-linux-2.0.3-amd64.deb" | sha256sum -c
```

看到 `OK` 说明文件完整无损，可以删除分卷文件了。

## 3. 安装

在安装包所在目录执行（`apt` 会自动处理依赖）：

```bash
# amd64
sudo apt install ./clash-party-linux-2.0.3-amd64.deb

# arm64
sudo apt install ./clash-party-linux-2.0.3-arm64.deb
```

## 4. 使用

1. 从系统应用菜单启动 Clash Party。
2. 进入「订阅」页面，添加你的订阅链接并更新，或者直接导入本地配置文件。
3. 在「代理」页面选择节点。
4. 打开「系统代理」开关，让浏览器和命令行走代理。
5. 如需代理全部流量（含不读取系统代理的程序），可开启 TUN 模式；该模式需要授权，按界面提示操作即可。

## 5. 卸载

```bash
sudo apt remove clash-party
```
