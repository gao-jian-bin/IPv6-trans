# 校园网 IPv6 云服务器申请与 Hysteria 2 节点配置教程

本文档将带你完成以下流程：

- GitHub Education 学生认证
- 领取 GitHub Student Developer Pack 中的云服务权益
- 在 DigitalOcean 创建一台开启 IPv6 的服务器
- 登录服务器并完成基础初始化
- 获取 Hysteria 2 服务端信息并生成客户端节点

---

## 目录

- [一、准备内容](#一准备内容)
- [二、方案 A：GitHub Education + DigitalOcean](#二方案-agithub-education--digitalocean)
- [三、方案 B：直接购买服务器](#三方案-b直接购买服务器)
- [四、创建服务器](#四创建服务器)
- [五、登录服务器并完成客户端配置](#五登录服务器并完成客户端配置)

---

## 一、准备内容

开始前请准备好以下内容：

1. 一个 GitHub 账号
2. 学校教育邮箱，或可证明在读身份的正式材料
3. 一台本地电脑
4. 可正常访问 GitHub 的网络环境

---

## 二、方案 A：GitHub Education + DigitalOcean

如果你具备学生身份，可以先申请 GitHub Education，再领取 Student Developer Pack 中的云服务权益。

### 1）申请 GitHub Education

- 官方入口：
  <https://github.com/settings/education/benefits>
- 参考教程：
  <https://zhuanlan.zhihu.com/p/578964972>

打开后按页面要求提交材料即可。可使用以下任一材料：

- 学校教育邮箱
- 学生证
- 学籍证明
- 其他正式在读证明

如果上传页面异常，尽量使用稳定网络环境重新提交。

---

### 2）进入 Student Developer Pack

- 入口：
  <https://education.github.com/pack>

完成学生认证后，进入 Pack 页面查看可领取的合作权益。

点击对应云服务后，按页面步骤完成绑定和授权。

![GitHub Student Pack 页面](image/github-student-pack.png)

---

### 3）绑定 DigitalOcean

根据页面提示完成授权与账户绑定。

下面是示例流程图：

![选择支付方式](image/payment-method.png)

![绑定流程步骤 1](image/binding-step-1.png)

![绑定流程步骤 2](image/binding-step-2.png)

绑定成功后，回到 DigitalOcean 控制台，在左侧导航栏打开：

`Billing`

确认权益是否已经到账。

![Billing 页面查看额度](image/billing-credit.png)

---

## 三、方案 B：直接购买服务器

如果你暂时没有学生认证，或者不想等待审核，也可以直接在正规云平台购买一台支持 IPv6 的服务器。

买完后，后续步骤与下文相同，直接从 **[四、创建服务器](#四创建服务器)** 开始即可。

---

## 四、创建服务器

下面以 DigitalOcean 为例。

### 1）创建 Droplet

进入控制台后，创建一台新的服务器实例。

![创建实例](image/create-droplet.png)

---

### 2）选择系统镜像

选择常见的 Linux 发行版即可，推荐 Ubuntu LTS。

![选择系统镜像](image/choose-os-image.png)

---

### 3）选择配置

常规学习和自建服务场景，选择价格较低的配置即可。

![选择低价配置](image/choose-plan.png)

---

### 4）设置登录方式

可以设置密码，也可以使用 SSH Key。
如果当前只是想先快速完成创建，先设置密码即可。

![设置登录密码](image/set-login-password.png)

---

### 5）开启 IPv6

这一步非常关键：**创建实例时务必勾选 IPv6**。

![勾选 IPv6](image/enable-ipv6.png)

确认无误后继续创建。

![确认创建实例](image/confirm-create-droplet.png)

---

### 6）记录服务器 IPv6 地址

实例创建完成后，会在详情页看到服务器分配到的 IP 地址。

<a id="remember-ipv6"></a>

> [!IMPORTANT]
> 请记住这里显示的 **IPv6 地址**，后面生成客户端配置时会用到。

![查看实例 IPv6 地址](image/view-ipv6-address.png)

---

## 五、登录服务器并完成客户端配置

### 1）打开控制台

在实例页面打开服务器控制台。

![打开控制台](image/open-console.png)

进入后即可看到终端界面。

![进入控制台终端](image/console-terminal.png)

---

### 2）更新系统并安装 Hysteria 2

登录成功后，先更新系统软件包：

```bash
sudo apt update
sudo apt upgrade -y
```

然后执行 Hysteria 2 安装脚本：

```bash
wget -N --no-check-certificate https://raw.githubusercontent.com/flame1ce/hysteria2-install/main/hysteria2-install-main/hy2/hysteria.sh && bash hysteria.sh
```

安装过程中按下面的方式填写即可。

选择 `1`：
![选择安装选项 1](image/hy2-install-select-1.png)

选择 `Yes`：
![确认继续安装](image/hy2-install-select-yes.png)

以下几步如果没有特殊需求，保持默认配置即可：
![保持默认配置步骤 1](image/hy2-install-default-step-1.png)
![保持默认配置步骤 2](image/hy2-install-default-step-2.png)
![保持默认配置步骤 3](image/hy2-install-default-step-3.png)

密码可以直接使用随机生成的值：
![使用随机密码](image/hy2-install-random-password.png)

`SNI` 这里填写 `www.bing.com` 即可：
![SNI 填写示例](image/hy2-install-sni-bing.png)

---

### 3）复制 Hysteria 2 服务端信息

拿到 Hysteria 2 服务端信息后，就可以开始生成客户端配置并导入使用。

部署完成后，通常会得到一组类似下图的服务端信息：

![Hysteria 2 服务端信息示例](image/hysteria2-server-info.png)

请将这些信息完整复制保存，后续生成客户端节点时会直接用到。

建议重点确认以下内容是否齐全：

- 服务器 IPv6 地址
- 端口
- 认证信息（`auth` / 密码）
- TLS 相关信息
- `SNI`
- 其他传输参数

如果缺少关键字段，先补齐后再继续生成配置。

---

### 4）让 AI 生成可用节点

将上一步复制到的 Hysteria 2 服务端信息发给 `AI`，以及我们前面的 [`IPv6`](#remember-ipv6) 地址，并使用下面这段指令：

```text
请根据我提供的 Hysteria 2 服务端信息，生成一份仅使用 IPv6 地址的客户端配置，不要使用 IPv4。请同时输出：
1）可直接使用的 Hysteria 2 YAML 配置；
2）可供 v2rayN 导入的连接格式（URI、JSON 或其他兼容格式）。
要求配置字段完整、格式准确，包含 server、port、auth、tls、sni 及相关传输参数；如果存在缺失项，请先明确指出需要补充的参数，再生成最终配置。
```

这样可以让 `AI` 直接根据你提供的服务端信息生成可用配置，并明确要求仅使用 IPv6 地址。

如果 `AI` 提示某些参数缺失，请先回到服务端信息中补全，再重新生成最终配置。

---

### 5）导入客户端

如果使用的是 `Clash` 系列客户端，请新建一个 `YAML` 配置文件，将提供的配置代码完整复制进去后再导入客户端。

如果使用的是其他代理客户端，例如 `v2rayN`，请按照指定的配置格式直接复制并粘贴导入。

---

### 6）启用代理

节点导入完成后，启用该节点即可开始使用。

使用方式任选其一：

- 开启 `TUN` 模式
- 使用系统代理 + 全局模式

以上两种方式都可以，按自己的使用习惯选择即可。

---

## 相关链接

- GitHub Education  
  <https://github.com/settings/education/benefits>

- GitHub Student Developer Pack  
  <https://education.github.com/pack>

- DigitalOcean  
  <https://www.digitalocean.com/>
