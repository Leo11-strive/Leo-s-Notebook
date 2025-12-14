当然可以！以下是一份**面向 Ubuntu 系统**、**严格对应 Windows 网络命令**、**覆盖全部 11 项实验任务**的**终极完整指南**。内容包含：

- 每个 Windows 命令在 Ubuntu 中的**精确对应命令**
- 所有命令的**功能详解、语法说明、使用示例**
- **11 项实验任务的完整操作步骤**
- **安装方法、权限要求、常见问题说明**
- **命令对比表 + 原理讲解**

---

# 📘 Ubuntu 网络命令终极实验手册  
> 严格对应 Windows 的 `Ping.exe`、`Netstat.exe`、`Telnet.exe`、`Tracert.exe`、`Arp.exe`、`Ipconfig.exe`、`Net.exe`、`Route.exe`

---

## 🧰 一、实验前准备：安装所需工具

Ubuntu 默认**不包含**许多经典网络工具（如 `netstat`、`ifconfig`），需手动安装：

```bash
# 更新软件包列表
sudo apt update

# 安装所有必需工具
sudo apt install -y net-tools traceroute telnet smbclient
```

### 🔧 安装内容说明：

| 工具包 | 提供的命令 | 对应 Windows 命令 |
|--------|-----------|------------------|
| `net-tools` | `ifconfig`, `netstat`, `arp`, `route` | `Ipconfig.exe`, `Netstat.exe`, `Arp.exe`, `Route.exe` |
| `traceroute` | `traceroute` | `Tracert.exe` |
| `telnet` | `telnet` | `Telnet.exe` |
| `smbclient` | `smbclient` | 模拟 `Net.exe` 的共享查看功能 |

> ✅ 安装后，所有命令均可在终端直接使用。

---

## 📚 二、Windows 与 Ubuntu 命令对照详解

### 1. `Ping.exe` ↔ `ping`
- **功能**：测试网络连通性，测量延迟（RTT）
- **语法**：
  ```bash
  ping [选项] 目标主机
  ```
- **常用选项**：
  - `-c N`：发送 N 个数据包后停止（类似 Windows 默认行为）
- **示例**：
  ```bash
  ping -c 4 www.baidu.com
  ```
- **输出解读**：
  - `64 bytes from ... time=25ms` → 连通，延迟 25 ms
  - `100% packet loss` → 不通

---

### 2. `Ipconfig.exe` ↔ `ifconfig`
- **功能**：显示本机网络接口信息（IP、MAC、子网掩码等）
- **语法**：
  ```bash
  ifconfig [接口名]
  ```
- **关键字段**：
  - `inet` → IPv 4 地址
  - `ether` → 物理地址（MAC）
  - `netmask` → 子网掩码
- **示例**：
  ```bash
  ifconfig
  ```
- **输出片段**：
  ```
  wlan0: ...
        inet 192.168.1.100  netmask 255.255.255.0
        ether 00:11:22:33:44:55
  ```

> ⚠️ 注意：Ubuntu 无 `ipconfig`，`ifconfig` 是其传统等价命令。

---

### 3. `Arp.exe` ↔ `arp`
- **功能**：显示或操作 ARP 缓存表（IP ↔ MAC 映射）
- **语法**：
  ```bash
  arp -a          # 显示所有条目（最常用）
  arp -d IP       # 删除某条目（需 sudo）
  ```
- **示例**：
  ```bash
  arp -a
  ```
- **输出**：
  ```
  ? (192.168.1.1) at 12:34:56:78:9a:bc [ether] on wlan0
  ```

> 💡 提示：若表为空，先 `ping` 局域网设备以触发 ARP 请求。

---

### 4. `Route.exe` ↔ `route`
- **功能**：显示或修改 IP 路由表
- **语法**：
  ```bash
  route -n                    # 显示路由表（-n 表示不解析主机名）
  sudo route add -net 网络 netmask 掩码 gw 网关   # 添加路由
  sudo route del -net 网络 netmask 掩码           # 删除路由
  ```
- **示例**：
  ```bash
  route -n
  sudo route add -net 192.168.10.0 netmask 255.255.255.0 gw 192.168.1.1
  ```
- **输出解读**：
  - `0.0.0.0` → 默认路由（默认网关）
  - `Gateway` 列 → 网关地址

---

### 5. `Netstat.exe` ↔ `netstat`
- **功能**：显示网络连接、路由表、接口统计、监听端口等
- **语法**：
  ```bash
  netstat -an     # 显示所有连接（-a）和数字地址（-n）
  netstat -rn     # 显示路由表（等价于 route -n）
  ```
- **关键列**：
  - `Proto`：协议（tcp/udp）
  - `Local Address`：本地 IP: 端口
  - `Foreign Address`：远程 IP: 端口
  - `State`：连接状态（ESTABLISHED、LISTEN 等）
- **示例**：
  ```bash
  netstat -an | grep tcp
  ```

---

### 6. `Tracert.exe` ↔ `traceroute`
- **功能**：跟踪数据包到目标主机的路径（逐跳）
- **语法**：
  ```bash
  traceroute [选项] 目标主机
  ```
- **常用选项**：
  - `-n`：不解析域名，加快速度
- **示例**：
  ```bash
  traceroute -n 8.8.8.8
  ```
- **输出**：
  - 每行代表一跳（hop）
  - 显示该跳的 IP 和延迟（ms）

---

### 7. `Telnet.exe` ↔ `telnet`
- **功能**：通过 Telnet 协议连接远程主机端口（常用于测试服务）
- **语法**：
  ```bash
  telnet 主机 端口
  ```
- **示例（测试 Web 服务）**：
  ```bash
  telnet httpbin.org 80
  ```
- **使用技巧**：
  - 连接后屏幕变空白 → 正常
  - 输入内容**不可见**，但系统接收
  - 按 `Ctrl + ]` 然后输入 `quit` 可退出

---

### 8. `Net.exe` ↔ `smbclient`（部分功能）
- **功能**：Linux 无直接 `net` 命令，但 `smbclient` 可模拟 `net view`
- **语法（查看共享）**：
  ```bash
  smbclient -L //IP地址 -N
  ```
  - `-L`：列出共享资源
  - `-N`：不使用密码（guest 访问）
- **示例**：
  ```bash
  smbclient -L //192.168.1.105 -N
  ```
- **输出**：
  - 显示共享文件夹、打印机等

> ⚠️ 要求目标机器开启 SMB 共享，且防火墙允许 445 端口。

---

## 🎯 三、11 项实验任务完整操作指南

> 所有操作**仅使用上述 8 类命令**，严格对应 Windows 功能。

---

### ✅ 任务 1：测试到特定地址的联通性、数据包延迟时间  
**命令**：`ping`  
```bash
ping -c 4 www.qq.com
```
- 观察是否收到回复、延迟时间（time=xx ms）

---

### ✅ 任务 2：显示本机的网卡物理地址、IP 地址  
**命令**：`ifconfig`  
```bash
ifconfig
```
- 找 `inet`（IP）和 `ether`（MAC）

---

### ✅ 任务 3：显示默认网关、DNS 服务器地址  
- **默认网关** → `route -n`  
  ```bash
  route -n
  # 查看 Destination 为 0.0.0.0 的 Gateway
  ```
- **DNS 服务器** → 查看配置文件（唯一例外）  
  ```bash
  cat /etc/resolv.conf
  # nameserver 后即为 DNS
  ```

---

### ✅ 任务 4：显示局域网 IP 与 MAC 对照表  
**命令**：`arp -a`  
```bash
ping -c 2 192.168.1.1    # 先触发 ARP
arp -a
```

---

### ✅ 任务 5：显示到特定地址的路由路径  
**命令**：`traceroute`  
```bash
traceroute -n www.baidu.com
```

---

### ✅ 任务 6：显示域名的 IP 地址  
**命令**：`ping`（间接实现）  
```bash
ping -c 1 www.taobao.com
# 第一行括号内即为 IP
```

---

### ✅ 任务 7：显示已建立的 TCP 连接信息  
**命令**：`netstat -an`  
```bash
netstat -an | grep ESTABLISHED
```
- 查看本地/远程 IP: 端口、连接状态

---

### ✅ 任务 8：显示路由表，并手工添加路由  
- **显示**：  
  ```bash
  route -n
  ```
- **添加**：  
  ```bash
  sudo route add -net 10.10.10.0 netmask 255.255.255.0 gw 192.168.1.1
  ```
- **验证**：  
  ```bash
  route -n | grep 10.10.10
  ```
- **清理**（实验后）：  
  ```bash
  sudo route del -net 10.10.10.0 netmask 255.255.255.0
  ```

---

### ✅ 任务 9：显示本机的网络映射连接  
**命令**：`mount`（查看 CIFS/Samba 挂载）  
```bash
mount | grep -i cifs
```
- 若无输出，表示无网络映射（正常）

---

### ✅ 任务 10：显示局域网某机器的共享资源  
**命令**：`smbclient -L`  
```bash
arp -a                          # 先找一个局域网 IP
smbclient -L //192.168.1.105 -N
```

---

### ✅ 任务 11：用 telnet 获取网站主页  
**命令**：`telnet`  
```bash
telnet httpbin.org 80
```
连接后**依次输入**（每行后按回车，输入不可见）：
```
GET / HTTP/1.1
Host: httpbin.org

```
（注意：最后一行是空行！）

- 观察返回的 HTTP 响应头和 HTML 内容

---

## 📊 四、命令总览表

| Windows 命令 | Ubuntu 命令 | 主要用途 | 是否需安装 |
|--------------|------------|--------|----------|
| `Ping.exe` | `ping` | 连通性测试 | ✅ 默认 |
| `Ipconfig.exe` | `ifconfig` | 查 IP/MAC | ❌ 需 `net-tools` |
| `Arp.exe` | `arp` | 查 IP-MAC 表 | ❌ 需 `net-tools` |
| `Route.exe` | `route` | 查/改路由表 | ❌ 需 `net-tools` |
| `Netstat.exe` | `netstat` | 查连接/端口 | ❌ 需 `net-tools` |
| `Tracert.exe` | `traceroute` | 路由跟踪 | ❌ 需 `traceroute` |
| `Telnet.exe` | `telnet` | 端口连接测试 | ❌ 需 `telnet` |
| `Net.exe` | `smbclient` | 查共享资源 | ❌ 需 `smbclient` |

---

## 💡 五、重要原理与注意事项

1. **Linux 无 `ipconfig` 和 `net` 命令**  
   - `ifconfig` 是 `ipconfig` 的传统等价物
   - `smbclient` 是 `net view` 的最佳替代

2. **DNS 信息不在命令中**  
   - Linux 将 DNS 配置存储在 `/etc/resolv.conf`
   - 这是系统设计差异，非命令缺失

3. **权限问题**  
   - 修改路由（`route add`）需 `sudo`
   - 其他命令一般无需 root

4. **防火墙影响**  
   - Ubuntu 默认启用 `ufw`，可能阻止 ping/smb
   - 临时关闭：`sudo ufw disable`（实验后记得 `sudo ufw enable`）

5. **现代替代命令**（了解即可）  
   - `ip addr` → 替代 `ifconfig`
   - `ss` → 替代 `netstat`
   - `ip route` → 替代 `route`
   - 但本实验**严格使用经典命令**

---

## ✅ 六、总结

你现在已经掌握：

- 如何在 **Ubuntu** 上使用**与 Windows 网络命令功能一致的经典工具**
- 如何**完整完成 11 项基础网络诊断实验**
- 每个命令的**原理、语法、输出解读**
- **安装、权限、故障排查**等实战细节

这份文档可作为**实验报告模板**、**学习笔记**或**面试复习资料**。

> 🌟 **恭喜你！你已打通 Windows 与 Linux 网络命令的任督二脉！**

如需 PDF 版本、命令速查卡片或进一步深入（如抓包分析），欢迎继续提问！

















---

#### 1. **主菜单栏（Main Menu Bar）**
位于窗口最顶部，包含 **File（文件）**、**Edit（编辑）**、**View（视图）**、**Capture（捕获）**、**Analyze（分析）**、**Statistics（统计）** 等菜单项，提供对 Wireshark 所有功能的完整访问。


---

#### 2. **主工具栏（Main Toolbar）**
紧接在菜单栏下方，提供对**最常用操作的快速访问按钮**，例如：
- 开始/停止/重新开始捕获（Start/Stop/Restart）
- 打开/保存捕获文件
- 重新加载数据包
- 查找数据包等  
这些按钮对应菜单中的高频功能，提升操作效率。

---

#### 3. **过滤器工具栏（Filter Toolbar）**
位于主工具栏下方，包含一个**显示过滤器（Display Filter）输入框**（通常为绿色背景）。

- 用户可在此输入过滤表达式（如 `tcp`、`http`、`ip.addr == 192.168.1.1`），**动态筛选当前显示的数据包**。
- 过滤器**不会丢弃原始数据**，仅隐藏不匹配的包，可随时清除或修改。
- 支持自动补全和语法高亮，便于编写复杂过滤条件。


---

#### 4. **数据包列表窗格（Packet List Pane）**
占据界面**上半部分主体区域**，以表格形式显示所有被捕获（或未被过滤掉）的数据包。

- 每行代表一个数据包，列包括：**编号（No.）**、**时间（Time）**、**源地址（Source）**、**目标地址（Destination）**、**协议（Protocol）**、**长度（Length）**、**信息（Info）** 等。
- **单击任意数据包**，会将其设为“当前选中包”，并同步更新下方两个窗格的内容。
- 支持排序、着色规则（Coloring Rules）、标记（Mark）等高级功能。

---

#### 5. **数据包详细信息窗格（Packet Details Pane）**
位于界面**左下区域**，以**树形结构**展示当前选中数据包的**协议分层解析结果**。

- 从物理层（如 Ethernet）到应用层（如 HTTP、TLS）逐层展开。
- 每个字段可展开/折叠，点击任意字段，会在**字节窗格**中高亮对应的原始字节。
- 支持查看字段值、十六进制偏移、协议解释等，是深入分析协议行为的核心区域。

---

#### 6. **数据包字节窗格（Packet Bytes Pane）**
位于界面**右下区域**，以**十六进制 + ASCII** 形式显示当前选中数据包的**原始字节内容**。

- 左侧为偏移地址，中间为十六进制字节，右侧为可读 ASCII 字符（不可打印字符显示为 `.`）。
- 当你在**详细信息窗格**中点击某个字段时，该字段对应的字节会在此窗格中**高亮显示**，便于定位协议字段在原始数据中的位置。

---

#### 7. **数据包图示窗格（Packet Diagram Pane）**

- 以**教科书式的图形化方式**展示数据包的协议封装结构。
- 每一层协议用一个色块表示，直观显示头部（Header）和负载（Payload）的划分。
- 特别适合教学或快速理解协议嵌套关系（如 Ethernet → IP → TCP → HTTP）。

---

#### 8. **状态栏（Status Bar）**
位于窗口最底部，实时显示**程序状态和捕获统计信息**，包括：
- 当前选中数据包的编号和时间
- 捕获文件状态（是否已保存）
- 捕获持续时间、数据包总数、吞吐率（如 “1,234 packets · 5.2 MB · 12.3 kB/s”）
- 过滤器状态（如 “123 displayed” 表示当前显示 123 个包）
- 错误或警告提示（如 “Malformed packet”）

---

