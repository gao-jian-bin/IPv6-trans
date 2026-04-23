# 校园网 IPv6 云服务器申请与基础测试教程

本文档用于记录以下内容：

- GitHub Education 学生认证
- 领取 GitHub Student Developer Pack 中的云服务权益
- 在 DigitalOcean 创建一台开启 IPv6 的服务器
- 登录服务器并完成基础初始化
- 进行 IPv6 连通性测试

---

## 目录

- [一、准备内容](#一准备内容)
- [二、方案 A：GitHub Education + DigitalOcean](#二方案-agithub-education--digitalocean)
- [三、方案 B：直接购买服务器](#三方案-b直接购买服务器)
- [四、创建服务器](#四创建服务器)
- [五、登录服务器](#五登录服务器)
- [六、初始化服务器](#六初始化服务器)
- [七、测试 IPv6 是否正常](#七测试-ipv6-是否正常)

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

官方入口：

<https://github.com/settings/education/benefits>
GitHub 学生认证教程：
<https://zhuanlan.zhihu.com/p/578964972>

打开后按页面要求提交材料即可。  
可使用以下任一材料：

- 学校教育邮箱
- 学生证
- 学籍证明
- 其他正式在读证明

如果上传页面异常，尽量使用稳定网络环境重新提交。

---

### 2）进入 Student Developer Pack

入口：

<https://education.github.com/pack>

完成学生认证后，进入 Pack 页面查看可领取的合作权益。

点击对应云服务后，按页面步骤完成绑定和授权。

![GitHub Student Pack 页面](https://github.com/user-attachments/assets/b14240c5-d371-4c34-99cd-8d0d20a6fbed)

---

### 3）绑定 DigitalOcean

根据页面提示完成授权与账户绑定。

下面是示例流程图：

![选择支付方式](https://github.com/user-attachments/assets/7747c61f-ef30-475f-ae3b-b649a936de52)

![绑定流程步骤 1](https://github.com/user-attachments/assets/010fe415-498c-4dee-80c0-8912814ebe2d)

![绑定流程步骤 2](https://github.com/user-attachments/assets/f206fba2-2c5d-4fad-b91b-d46b7e39ba6b)

绑定成功后，回到 DigitalOcean 控制台，在左侧导航栏打开：

`Billing`

确认权益是否已经到账。

![Billing 页面查看额度](https://github.com/user-attachments/assets/17b034b0-f39f-42cd-acef-ad11aae1dada)

---

## 三、方案 B：直接购买服务器

如果你暂时没有学生认证，或者不想等待审核，也可以直接在正规云平台购买一台支持 IPv6 的服务器。

买完后，后续步骤与下文相同，直接从 **[四、创建服务器](#四创建服务器)** 开始即可。

---

## 四、创建服务器

下面以 DigitalOcean 为例。

### 1）创建 Droplet

进入控制台后，创建一台新的服务器实例。

![创建实例](https://github.com/user-attachments/assets/5676ec42-4862-44a1-a61f-77ac43f563d4)

---

### 2）选择系统镜像

选择常见的 Linux 发行版即可，推荐 Ubuntu LTS。

![选择系统镜像](https://github.com/user-attachments/assets/9b2d7704-498f-4314-a9dd-afee76f7b279)

---

### 3）选择配置

常规学习和基础测试场景，选择价格较低的配置即可。

![选择低价配置](https://github.com/user-attachments/assets/39a81b82-8f1c-4346-a128-382efee9b646)

---

### 4）设置登录方式

可以设置密码，或者使用 SSH Key。  
如果当前只是先快速完成创建，先设置密码也可以。

![设置登录密码](https://github.com/user-attachments/assets/91836ac1-a738-4d5f-9885-e648b92ebdd1)

---

### 5）开启 IPv6

这一步非常关键：**创建实例时务必勾选 IPv6**。

![勾选 IPv6](https://github.com/user-attachments/assets/be815391-720d-4340-8901-866517e75c7e)

确认无误后继续创建。

![确认创建实例](https://github.com/user-attachments/assets/eb4dd472-fac9-4ab2-9807-f89ccff39202)

---

### 6）记录服务器 IPv6 地址

实例创建完成后，会在详情页看到服务器分配到的 IP 地址。  
这里请记住你的 **IPv6 地址**，后面测试时会用到。

![查看实例 IPv6 地址](https://github.com/user-attachments/assets/3091efad-6523-41cc-9424-9c208e3db09e)

---

## 五、登录服务器

### 1）打开控制台

在实例页面打开服务器控制台。

![打开控制台](https://github.com/user-attachments/assets/ce6ac10d-7969-4a19-a625-80dc4c237dbc)

进入后即可看到终端界面。

![进入控制台终端](https://github.com/user-attachments/assets/d08de22d-3472-4fbf-a85b-e0b6fabfa816)

---

### 2）通过 SSH 登录（可选）

如果你想在本地终端登录，也可以直接使用 SSH。

使用 IPv4 登录：

```bash
ssh root@你的服务器IPv4
```

使用 IPv6 登录：

```bash
ssh root@[你的IPv6地址]
```

例如：

```bash
ssh root@[2400:xxxx:xxxx:xxxx::1]
```

---

## 六、初始化服务器

登录成功后，先更新系统软件包。

```bash
sudo apt update
sudo apt upgrade -y
```

如果你使用的是 Ubuntu，这两条命令执行完成后，系统基础环境就更新好了。

---

### 1）安装常用工具

```bash
sudo apt install -y curl wget vim git dnsutils traceroute
```

---

### 2）查看网卡和地址信息

```bash
ip addr
ip -6 addr
```

如果已经正确分配 IPv6，你会在输出中看到全局 IPv6 地址。

---

### 3）查看路由信息

```bash
ip route
ip -6 route
```

---

## 七、测试 IPv6 是否正常

完成初始化后，执行下面几项基础测试。

### 1）查看当前服务器 IPv6 地址

```bash
ip -6 addr
```

---

### 2）测试 IPv6 连通性

可以先测试公共 IPv6 DNS 地址：

```bash
ping -6 2606:4700:4700::1111
```

如果网络正常，会看到持续返回 `64 bytes from ...` 的结果。

---

### 3）测试 IPv6 出口

```bash
curl -6 https://api64.ipify.org
```

如果返回的是一个 IPv6 地址，说明当前服务器 IPv6 出口正常。

---

### 4）测试 AAAA 记录解析

```bash
dig AAAA google.com
```

如果解析成功，会看到 `AAAA` 记录结果。

---

### 5）测试 IPv6 路由路径

```bash
traceroute -6 2606:4700:4700::1111
```

如果能够正常返回多跳信息，说明 IPv6 路由已建立。

---

## 完成

到这里，你已经完成了以下内容：

- 申请 GitHub Education（可选）
- 领取 Student Developer Pack 权益（可选）
- 创建一台开启 IPv6 的云服务器
- 登录服务器
- 更新系统并安装基础工具
- 验证 IPv6 是否已经可以正常使用

后续如需部署你自己的学习环境，可以继续在这台服务器上进行常规开发和测试。

---

## 相关链接

- GitHub Education  
  <https://github.com/settings/education/benefits>

- GitHub Student Developer Pack  
  <https://education.github.com/pack>

- DigitalOcean  
  <https://www.digitalocean.com/>