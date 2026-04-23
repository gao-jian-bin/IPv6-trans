# 校园网IPv6转发教程

本仓库文档仅用于记录校园网 IPv6 的基础概念、连通性测试方法和合规使用建议。
# 免责声明

本文档仅用于 IPv6 基础知识、网络诊断和合规使用说明，不构成任何绕过认证、规避计费、规避审计或未授权访问的操作指南。


不提供也不鼓励以下行为：

- 绕过校园网计费
- 规避网络审计或监控
- 搭建或使用未授权的转发、代理、隧道、VPN 中继
- 借助 IPv6 出口为 IPv4 流量“套壳”或逃逸

如果你所在学校对 IPv6 提供了免费或优先接入策略，请以学校网络中心、信息化办公室或运营方公布的规则为准。

# 背景

全国绝大多数高校的校园网是不计入IPv6的上行和下行的流量，所以我们可以把我们的校园网的目的地址转换成一个IPv6的地方，这样就不会被计入流量上传与下载，那么所以现在就应该先准备一台服务器，把我们的校园网的流量转发都定位到这台服务器，然后利用这台服务器帮我们转发流量，这样我们就实现了所有的流量都走IPv6了

下面我们介绍两种方案，分别是零成本和有成本：

### 方案 A：低成本方案

如果你具备学生身份，可以优先尝试 GitHub Education 提供的学生权益。

基本思路：

1. 准备好 GitHub 账号
2. 准备学校教育邮箱，或可证明在读身份的正式材料
3. 通过 GitHub Education 提交学生认证
4. 认证通过后，查看 Student Developer Pack 中可领取的合作权益

**学生认证准备**

GitHub Education 官方认证入口：<https://github.com/settings/education/benefits>

然后按照要求来就行了，这里介绍一个秒过的方案，这样就不是零成本了，也就是从学信网上申请一个全英文的学籍证明，然后上传到GitHub这里的认证处，如果期间发现只能用摄像头不能上传文件的话建议**关闭代理**使用一下

**获取 DigitalOcean (DO) 免费服务器**

有了 GitHub 学生认证以后，就可以来到 DigitalOcean (DO) 这个知名的云服务平台来获取免费的服务器了。

操作步骤：
点击这个链接 https://education.github.com/pack
进行授权
<img width="1910" height="915" alt="9b569454a3696266474efb0fe8cd8d3e" src="https://github.com/user-attachments/assets/b14240c5-d371-4c34-99cd-8d0d20a6fbed" />

这里可以选择支付宝，有其他方式也可以，下面以支付宝为例
<img width="1920" height="910" alt="e0fd30fc2ba82658ec6fc5340836cbe4" src="https://github.com/user-attachments/assets/7747c61f-ef30-475f-ae3b-b649a936de52" />

<img width="1920" height="910" alt="d4785bbabf13a9d765564f000ae777aa" src="https://github.com/user-attachments/assets/010fe415-498c-4dee-80c0-8912814ebe2d" />

<img width="1920" height="910" alt="8dcc880a7739b9be276f1d8dae2f8419" src="https://github.com/user-attachments/assets/f206fba2-2c5d-4fad-b91b-d46b7e39ba6b" />

成功以后，回到DO平台能够看到已经成功获得GitHub学生包的200刀的权益（点击左侧导航栏的`Billing`）
<img width="1920" height="910" alt="325c2052dd7d7556eeacefc81d1ba8fb" src="https://github.com/user-attachments/assets/17b034b0-f39f-42cd-acef-ad11aae1dada" />
然后我们创建实例
<img width="1920" height="910" alt="65f1e14cb74cd29bd4c90b3420ca30f4" src="https://github.com/user-attachments/assets/5676ec42-4862-44a1-a61f-77ac43f563d4" />

<img width="1920" height="910" alt="d93a4157261e4bcacaf275ded8a50684" src="https://github.com/user-attachments/assets/9b2d7704-498f-4314-a9dd-afee76f7b279" />

尽量选价格低的，流量上传下载跟配置没啥关系，所以200刀尽可能让他保证够用
<img width="1920" height="910" alt="dadeaef7f8910ac55a1439a383940152" src="https://github.com/user-attachments/assets/39a81b82-8f1c-4346-a128-382efee9b646" />

创建密码    密码随便
<img width="1920" height="910" alt="31dd1ae89986b6fb364c6a62914a8e41" src="https://github.com/user-attachments/assets/91836ac1-a738-4d5f-9885-e648b92ebdd1" />

**最重要的一步： 勾选IPv6！！！** 

<img width="1920" height="910" alt="219b74af97a1ab896116ad8c509c080b" src="https://github.com/user-attachments/assets/be815391-720d-4340-8901-866517e75c7e" />

<img width="1920" height="910" alt="a703812aaabff9821d59ebd09de406a9" src="https://github.com/user-attachments/assets/eb4dd472-fac9-4ab2-9807-f89ccff39202" />
创建好以后，会到这里，记住我们这里的`IPv6`的地址
<img width="1920" height="910" alt="a7fbb3028fe1a035e2ec68ee7ef0b43d" src="https://github.com/user-attachments/assets/3091efad-6523-41cc-9424-9c208e3db09e" />

打开这个

<img width="1920" height="910" alt="9acabedd020f482420727c675eaeb8db" src="https://github.com/user-attachments/assets/ce6ac10d-7969-4a19-a625-80dc4c237dbc" />
进入到里面
<img width="1295" height="1010" alt="91fa39aff9aef652754ff746514a9d00" src="https://github.com/user-attachments/assets/d08de22d-3472-4fbf-a85b-e0b6fabfa816" />

执行 
```bash
sudo apt update
sudo apt upgrade -y
```

```bash
wget -N --no-check-certificate https://raw.githubusercontent.com/flame1ce/hysteria2-install/main/hysteria2-install-main/hy2/hysteria.sh && bash hysteria.sh
```

选1
<img width="1296" height="930" alt="a72a9fc59af4b3bbe011df0db5c82b30" src="https://github.com/user-attachments/assets/a54718cb-f82a-464a-9f76-5f46f6e72881" />
下面都选`Yes`
<img width="1295" height="1010" alt="6ae28ac91c56ded82031f23bf9adf8f1" src="https://github.com/user-attachments/assets/491f3f24-4184-4ffe-868d-1f9f9b5476ec" />
下面默认就行
<img width="1295" height="1010" alt="59ea5dd420756a971e4858cdfd60decb" src="https://github.com/user-attachments/assets/3a784833-9232-49d2-aac1-46386befc117" />
<img width="1295" height="1010" alt="f7a4bfa6ae0b90f0b85323d0c538970c" src="https://github.com/user-attachments/assets/d18d3bf3-588c-4125-aea9-b12c8a904dbb" />
<img width="1295" height="1010" alt="0b7e677886bdb8b270757683772d0555" src="https://github.com/user-attachments/assets/a236d2de-4a67-4156-a01a-6758da7b55e6" />

密码随便设或者直接随机生成
<img width="1295" height="1010" alt="40d8c0fe5c2b48b6c5b7a5e82775f841" src="https://github.com/user-attachments/assets/0fc39b88-66fb-4201-8ac2-876afe63861c" />
混淆这里填入bing的, `www.bing.com`
<img width="1296" height="930" alt="8118fe70ccbc65cda4a39a1eef4b976b" src="https://github.com/user-attachments/assets/838e22a9-7a8a-440a-892b-86e41b21f264" />
会得到如下的信息
<img width="1422" height="1106" alt="image" src="https://github.com/user-attachments/assets/f9b38095-c886-4dd7-b03d-550e7887f6b8" />
我们把这些信息复制下来,然后去让`AI`生成一个能用的节点,指令如下:
```
请根据我提供的 Hysteria 2 服务端信息，生成一份仅使用 IPv6 地址的客户端配置，不要使用 IPv4。请同时输出：
1）可直接使用的 Hysteria 2 YAML 配置；
2）可供 v2rayN 导入的连接格式（URI、JSON 或其他兼容格式）。
要求配置字段完整、格式准确，包含 server、port、auth、tls、sni 及相关传输参数；如存在缺失项，请先明确指出需要补充的参数，再生成最终配置。
```

如果使用的是 `Clash` 系列客户端，请新建一个 `YAML` 配置文件，将提供的配置代码完整复制进去后再导入客户端。
如果使用的是其他代理客户端，例如 `v2rayN`，请按照指定的配置格式直接复制并粘贴导入。








### 方案 B：直接购买

如果你不想等待学生认证，或者当前没有可用的学生优惠，最直接的办法就是在正规云平台购买服务器。
买完以后后续同上

