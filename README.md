# 仅供高级用户使用的安装方法

## 请确保您理解[需求](https://github.com/home-assistant/architecture/blob/master/adr/0014-home-assistant-supervised.md)

# 安装 Home Assistant Supervised

该安装方法在常规操作系统上提供完整的 Home Assistant 使用体验。这意味着除了 Home Assistant 操作系统外，所有 Home Assistant 方法中的组件都会被使用。该系统将运行 Home Assistant Supervisor。Supervisor 不仅是一个应用程序，它是一个完整的设备，用于管理整个系统。如果设置不再符合预期值，它将清理、修复或重置设置为默认值。

由于不使用 Home Assistant 操作系统，用户需要负责确保安装和维护所有必需的组件。必需组件及其版本将随时间变化。Home Assistant Supervised 是按原样提供的，作为社区支持的自助解决方案的基础。我们仅接受在全新安装、完全更新的 Debian 系统上重现的错误报告，且不包含额外的软件包。

这种方法被认为是高级的，只有在熟悉管理 Linux 操作系统、Docker 和网络方面的专家才应使用。

本项目的目的是让在中国的homeassistant玩家更好的，不用在使用代理的情况下更快的完成安装（本项目还在更新中，可能还不能很好的投入使用）

## 计划
 - [√]修改supervisor容器的安装地址
 - [√]将网络检查地址改为百度
 - [×]使非Debian发行版安装如Ubuntu（不建议）
 - [×]汉化
 - [×]修改

## 安装

以 root 身份运行以下命令（对于已安装 sudo 的机器，使用 `su -` 或 `sudo su -`）：

步骤1：使用以下命令安装以下依赖项：

```bash
apt install \
apparmor \
jq \
wget \
curl \
udisks2 \
libglib2.0-bin \
network-manager \
dbus \
lsb-release \
systemd-journal-remote \
systemd-resolved -y
```

步骤2：使用以下命令安装 Docker-CE：

```bash
curl -fsSL get.docker.com | sh
```

步骤3：安装 OS-Agent：

有关安装 OS-Agent 的说明可以在[此处找到](https://github.com/home-assistant/os-agent/tree/main#using-home-assistant-supervised-on-debian)

步骤4：安装 Home Assistant Supervised Debian 包：
```bash
git clone https://github.com/yang6world/supervised-installer-inChain.git
cd supervised-installer-inChain
```

```bash
docker run --rm -v $(pwd):/tmp debian:bullseye-slim bash -c  "cd /tmp && chmod 555 homeassistant-supervised/DEBIAN/p* && dpkg-deb --build --root-owner-group homeassistant-supervised"
```

```bash
apt install ./homeassistant-supervised.deb
```

## 支持的机器类型

- generic-x86-64
- odroid-c2
- odroid-c4
- odroid-n2
- odroid-xu
- qemuarm
- qemuarm-64
- qemux86
- qemux86-64
- raspberrypi
- raspberrypi2
- raspberrypi3
- raspberrypi4
- raspberrypi3-64
- raspberrypi4-64
- tinker
- khadas-vim3

## 配置

我们的 `$DATA_SHARE` 的默认路径是 `/home/hassio`。
此路径用于存储与 Home Assistant 相关的所有内容。

您可以在安装过程中重新配置此路径，使用以下命令：

```bash
DATA_SHARE=/my/own/homeassistant dpkg --force-confdef --force-confold -i homeassistant-supervised.deb
```

## 故障排除

如果出现问题，请使用 `journalctl -f` 获取系统日志。如果您不熟悉 Linux 或如何解决问题，我们建议您使用我们的 Home Assistant 操作系统。