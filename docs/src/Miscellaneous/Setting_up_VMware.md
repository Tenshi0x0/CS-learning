# 配置虚拟机

## 作用

一个比较直白的原因是，需要 Linux 环境的作业会用到。

## 安装步骤

此处以 VMware + Ubuntu 24.04 为例。

1. 下载镜像：Ubuntu 官网下载对应 ISO 文件即可。
2. 下载 VMware（现在需要注册账号了）
3. 打开 VMware → 新建虚拟机 → 选择 ISO 文件。（推荐配置：2 CPU / 4–8 GB 内存 / 40 GB 硬盘。）

### 相关问题

#### 连不上网

比如 `ping -c 4 8.8.8.8` ping 不通（或者随便开一个网页）。

在主机（例如 Windows）打开 服务 (services.msc)，保证：

`VMware NAT Service, VMware DHCP Service`

状态列都显示“正在运行”。

> 可能还有其它原因导致无法联网，~~但我没遇到~~。

#### 没有全屏

```sh
sudo apt update
sudo apt install open-vm-tools open-vm-tools-desktop -y
sudo reboot
```

#### 缩放终端大小

放大：`Ctrl + Shift + +`

缩小：`Ctrl + -`

恢复默认：`Ctrl + 0`

上面的操作是一次性的，关掉就重置了，想要永久生效可以设置右上角的 `Preferences`：

<img src="/CS-learning/src/Miscellaneous/assets/vm1.png" alt="vm1" width="80%">

还有个放大整个 UI 文字的命令：

```sh
gsettings set org.gnome.desktop.interface text-scaling-factor 1.3
```

#### 显示隐藏文件

文件名或目录名前缀带一个点 `.` 时为隐藏文件。

用 `ctrl + H` 显示。

#### 全屏

`ctrl + alt + enter`