# 搬瓦工服务器教程与使用方法完整指南：从注册购买到 SSH 登录、KiwiVM 控制面板、宝塔建站，新手踩坑全避开

买了 VPS，却不知道怎么连上去——这应该是搬瓦工新手最常见的第一个坑。

这篇文章把搬瓦工服务器的教程和使用方法从头到尾捋了一遍：买哪个套餐、怎么登录 KiwiVM 面板、如何用 SSH 连上机器、装宝塔怎么操作、以及几个坑我踩过之后帮你标出来的地方。不追求面面俱到，追求你照着走下来不会卡住。

---

## 搬瓦工是什么，先用一段话搞清楚

搬瓦工（BandwagonHost）是来自加拿大 IT7 Networks 旗下的 VPS 服务商，做这行超过十年了，在中文用户圈里口碑一直不错。它的英文名 Bandwagonhost 因为发音有点像"搬瓦工"，这个名字就这么叫开了。

所谓 VPS，简单理解就是放在搬瓦工机房里的一台虚拟机，有自己的 IP、独立的 Linux 系统、独立的内存和硬盘。你用 SSH 连进去之后，操作和本地的 Linux 几乎一样——装软件、跑服务、搭网站，都可以。

它的核心优势两点：线路对中国大陆做了优化（CN2 GIA 这类高端线路是亮点），支付方式对国内用户友好，支付宝、银联都能付款。

👉 [查看搬瓦工最新套餐与当前折扣](https://bit.ly/BandWagonhost)

---

## 套餐选哪个：三档定位，别选错了

搬瓦工现在主要在售的常规套餐分三个档次，下面是完整汇总，按预算从低到高排：

| 套餐系列 | CPU | 内存 | 硬盘 | 月流量 | 带宽 | 价格 | 推荐机房 | 购买 |
|---|---|---|---|---|---|---|---|---|
| KVM / CN2 入门套餐 | 1 核 | 1 GB | 20 GB SSD | 1 TB | 1 Gbps | $49.99/年 | DC3/DC8 等 8 个 | [ 选择入门套餐](https://bandwagonhost.com/aff.php?aff=80865&pid=57) |
| KVM 大流量 B 套餐 | 2 核 | 1 GB | 40 GB SSD | 2 TB | 1 Gbps | $99.99/年 | DC2/DC4/DC8 | [ 选择此方案](https://bandwagonhost.com/aff.php?aff=80865&pid=58) |
| CN2 GIA-E 套餐（季付） | 2 核 | 1 GB | 20 GB SSD | 1 TB | 2.5 Gbps | $49.99/季 | DC6 等 11 个 | [ 选择 CN2 GIA-E](https://bandwagonhost.com/aff.php?aff=80865&pid=87) |
| CN2 GIA-E 套餐（年付） | 2 核 | 1 GB | 20 GB SSD | 1 TB | 2.5 Gbps | $169.99/年 | DC6/DC9 等 | [ 以年付价格购买](https://bandwagonhost.com/aff.php?aff=80865&pid=87) |
| CN2 GIA-E 大流量套餐 | 4 核 | 2 GB | 40 GB SSD | 2 TB | 2.5 Gbps | $299.99/年 | 15+ 机房可切换 | [ 选择大流量版](https://bandwagonhost.com/aff.php?aff=80865&pid=88) |
| 香港 CN2 GIA 套餐 A | 2 核 | 2 GB | 40 GB SSD | 500 GB | 1 Gbps | $899.99/年 | 香港 HK | [ 选择香港套餐](https://bandwagonhost.com/aff.php?aff=80865&pid=95) |
| 香港 CN2 GIA 套餐 B | 4 核 | 4 GB | 80 GB SSD | 1 TB | 2 Gbps | $1559.99/年 | 香港 HK | [ 选择香港高配](https://bandwagonhost.com/aff.php?aff=80865&pid=96) |

**怎么选？**

三种情况分开说：预算紧、只是学 Linux 或测试用途，CN2 入门套餐 $49.99/年够了；想建站、需要对国内访问速度有保证，直接上 CN2 GIA-E $169.99/年，算下来每天不到四毛钱，这个线路质量值这个价；预算充裕又对延迟极度敏感，香港套餐会让你满意，但价格也是量级上的差距。

限量版套餐这里不多说，偶尔出现、库存极少，遇到了合适就直接下手，不然等补货很费时间。

---

## 第一步：注册账号并完成购买

进官网之后点右上角 **Sign Up**，填邮箱和密码注册。注意用一个能正常收邮件的邮箱，因为购买完成后 VPS 信息会发到这里，之后重装系统也会发新密码到邮箱。

注册完成后选好套餐，进入结算页面。到 **Payment** 那一步：

1. 选支付方式——页面上有 **Alipay**（支付宝）、PayPal、银联等
2. 支付宝会跳出二维码，扫码付款即可，人民币结算
3. 付款后收到确认邮件，标题一般是 **"Payment Confirmation"** 或者 **"VPS Information"**

邮件里会有你的 VPS IP 地址、SSH 端口和初始 root 密码。**这个密码只会出现一次**，记下来或者立即改掉，关掉就找不到了，只能重置。

---

## 第二步：进入 KiwiVM 控制面板

KiwiVM 是搬瓦工官方自己做的 VPS 管理后台，每台 VPS 都有一个独立入口。

登录方法如下：

1. 进搬瓦工官网，点右上角 **Client Area** 登录账号
2. 进入后台后点 **Services → My Services**，看到你购买的 VPS
3. 点那台 VPS 后面的 **KiwiVM Control Panel** 按钮，跳转进去

进了 KiwiVM 之后，首页就能看到：

- **IP 地址**（Public IP Address）
- **SSH 端口**（SSH Port，搬瓦工的端口不是默认 22，是一个随机生成的五位数）
- VPS 当前状态（Running / Stopped）
- 流量使用情况

顺便提一嘴，KiwiVM 里还有这些常用功能：**Install new OS**（重装系统）、**Root password modification**（重置 root 密码）、**Root shell interactive**（浏览器直接开 SSH）、**Migrate to another DC**（迁移机房）。迁移机房这个功能 CN2 GIA-E 套餐可以用，但要注意迁移会换 IP 地址，迁前备份数据。

---

## 第三步：SSH 连接搬瓦工服务器

连 SSH 需要三个信息：IP 地址、端口号、root 密码。都在 KiwiVM 首页和购买确认邮件里。

### Windows 用户

推荐用 **Termius**，跨平台，免费版功能够用。下载安装后点 **+NEW HOST**，填入：

- Label（名字随便写）
- 服务器 IP 地址
- Port（SSH 端口，搬瓦工的是随机五位数，不是 22）
- Username 填 `root`
- Password 填你的 root 密码

保存之后点连接，第一次会弹出确认框选 Yes，然后就进去了。进去后看到命令行光标就是连上了。

除了 Termius，Xshell（有免费版）、PuTTY、PowerShell 都可以，原理一样。

### Mac 用户

打开终端（Terminal），直接输命令：

bash
ssh root@你的IP地址 -p 你的SSH端口


回车，输密码（注意输密码时屏幕不显示任何字符，这是正常的，输完直接回车）。

### Linux 用户

和 Mac 一样，终端直接跑上面那条命令。

不想装软件、临时用一下？KiwiVM 里有 **Root shell – interactive**，点 Launch，浏览器里就能直接操作 VPS，功能和 SSH 客户端一样。

---

## 第四步：装系统和基础配置

### 重装系统

搬瓦工 VPS 默认会装一个系统，如果你想换，在 KiwiVM 里操作：

1. 先点 **Stop** 把 VPS 关机
2. 进 **Install new OS** 页面
3. 选择系统版本（推荐 **Debian 12** 或 **Ubuntu 22.04**，稳定性好；如果要装宝塔面板，CentOS 也可以）
4. 勾选同意条款，点安装
5. 等邮件——重装完成后搬瓦工会发一封邮件包含新的 root 密码

关机这步**不能省**，没关机直接重装会失败。踩过的坑，写出来免得你也踩。

### 修改 root 密码

初始密码又长又随机，实际上在 KiwiVM 里可以用 **Root password modification** 功能重置成你自己记得住的密码。重置前同样要先关机。

---

## 第五步：安装宝塔面板（建站用）

如果你买搬瓦工是为了建站，宝塔面板是最省事的方案。它把 Nginx/Apache、PHP、MySQL、FTP 这些全用图形界面管理，不用天天敲命令行。

SSH 连上服务器后，运行对应的安装命令：

**Ubuntu / Debian 系统：**

bash
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh


**CentOS 系统：**

bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh


安装过程大概 3-5 分钟。装完后终端会显示宝塔的访问地址和初始账号密码，格式类似：


Bt-Panel: http://你的IP:8888
username: xxxxxxxx
password: xxxxxxxx


打开浏览器输入这个地址，登录进去，第一次会让你选 LNMP（Nginx + MySQL + PHP）还是 LAMP（Apache 版本）。建议选 LNMP，Nginx 比 Apache 更轻，小配置 VPS 跑起来更稳。

**注意一个细节**：宝塔默认用 8888 端口，如果浏览器访问不了，去 KiwiVM 的 **Firewall** 里把 8888 端口加到白名单。

---

## 关于机房选择：这是用搬瓦工最常被问到的问题

不同套餐能选的机房不一样。CN2 GIA-E 套餐可以后台一键在 15 个以上的机房之间迁移，算是最灵活的。

按线路质量从高到低大致是这个顺序：

- 香港 CN2 GIA / 日本东京 CMI → 延迟最低，亚洲精品线路
- 洛杉矶 DC6（CN2 GIA-E）/ DC9（CN2 GIA）→ 主流性价比选择，三网优化
- 日本大阪软银 → 移动用户体验好
- 洛杉矶 DC3/DC8、弗里蒙特 → 普通线路，入门够用

电信用户首选 DC6，联通和移动用户 DC6 也不错（三网都有回程优化）。如果你在用移动宽带，日本机房也值得考虑。

搬瓦工官网有每个机房的测试 IP，买之前先 ping 一下延迟，挑延迟低的那个机房，这个简单测试省很多后期折腾的时间。

---

## 搬瓦工能用来做什么

这个问题的标准答案是：一台 Linux VPS 能干的，搬瓦工都能干。

- **学习 Linux**：第一台 VPS，踩坏了重装，成本最低
- **搭建个人网站或博客**：WordPress 装上宝塔，十几分钟搭起来
- **部署应用**：Node.js、Python 各类后端服务
- **AI 相关**：最近用搬瓦工跑 n8n、Dify、OpenClaw 这类 AI Agent 应用的用户多了不少，中低配套餐 + 小模型可以跑起来
- **数据备份 / 文件中转**：装个 Nextcloud 或者简单的文件服务

对中国用户来说最大的特点还是线路——CN2 GIA 线路在晚高峰的稳定性比普通国际线路明显更好，这是搬瓦工用户口碑里被提到最多的一点。

用下来自己感受是：连接稳定性不错，遇到需要换 IP 或者迁机房这种情况，KiwiVM 里直接操作就行，不用提工单，这点挺省心的。另外 30 天内不满意可以全额退款，入手门槛其实没那么高。

👉 [查看搬瓦工全部套餐，选最适合你的方案](https://bit.ly/BandWagonhost)

---

## FAQ：搬瓦工常见问题快速解答

**Q：搬瓦工 SSH 端口是 22 吗？**
A：不是。搬瓦工的 SSH 端口是随机生成的五位数，在 KiwiVM 控制面板首页 SSH Port 那一行查看，每台 VPS 端口各不相同。

**Q：root 密码忘了怎么办？**
A：进 KiwiVM，先把 VPS 关机，然后用 Root password modification 功能重置。重置完会显示新密码，只出现一次，记住或者立即改掉。

**Q：搬瓦工买了可以换机房吗？**
A：CN2 GIA-E 套餐支持在 15 个以上机房之间自助迁移，在 KiwiVM 里找 Migrate to another DC 操作。迁移会导致 IP 变更，操作前备份重要数据。KVM/CN2 入门套餐只能在有限机房切换。

**Q：支付宝付款是人民币结算吗？**
A：是的。选支付宝后会显示人民币金额并生成二维码，直接扫码支付，不需要美元账户。

**Q：搬瓦工 VPS 可以安装 Windows 吗？**
A：官方不直接提供 Windows 镜像。理论上可以通过 DD 安装，但这属于高级操作，新手建议用 Linux。

**Q：流量用完了会怎样？**
A：流量耗尽后 VPS 会暂停网络，等下个月自动重置，或者在 KiwiVM 里付费购买额外流量。日常建站和轻度使用一般 1TB/月足够了。

---

## 写在最后

讲真，搬瓦工入门的步骤并不复杂——注册、选套餐、付款、进 KiwiVM 拿 SSH 信息、连上去、装你需要的服务。整个流程走下来快的话一小时以内搞定，慢一点也就两三个小时。

如果你是完全的新手，建议这个顺序：先买入门套餐练手，熟悉 Linux 基本操作之后，觉得线路不够快或者需求上来了，再升级到 CN2 GIA-E。年付比月付便宜一截，预算允许的话年付划算。

👉 [前往搬瓦工选购，当前价格直接显示最新折扣](https://bit.ly/BandWagonhost)
