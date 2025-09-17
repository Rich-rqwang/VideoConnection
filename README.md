# 程序安装说明

## Step 1 创建云服务器
- 购买一台云服务器（CVM），选择合适的实例类型和操作系统  
- 配置服务器的安全组规则，确保允许 **HTTP (80 端口)**、**HTTPS (443 端口)** 和应用使用的端口（如 **3000**）访问  

---

## Step 2 连接到服务器
通过 SSH 连接到你的腾讯云服务器。在本地终端或 SSH 客户端中运行以下命令：  

```bash
ssh root@your-server-ip
```

其中，`your-server-ip` 是你的服务器 IP 地址。初次登录时，可使用购买服务器时设置的密码或 SSH 密钥。  

---

## Step 3 安装 Git、Node.js 和 npm
在服务器上安装 Git、Node.js 和 npm：  

```bash
sudo yum install git -y
sudo yum install nodejs npm -y
```

---

## Step 4 获取项目代码
使用 Git 将项目代码克隆到服务器上：  

```bash
git clone https://github.com/Rich-rqwang/VideoConnection.git
cd VideoConnection
```

---

## Step 5 安装项目依赖
运行以下命令安装项目的所有依赖：  

```bash
npm ci
```

---

## Step 6 启动应用
先安装 SSL 自签名证书（详见 **Tips**），然后运行应用：  

```bash
node src/app.js
```

---

## Step 7 访问并测试
在浏览器中访问你的域名或 IP 地址，测试应用是否正常工作：  

```
https://your-server-ip:8090
```

---

## Tips
要实现视频和语音功能，由于安全问题需要保证访问地址为 **HTTPS**，而这需要 SSL 证书。  
生成自签名 SSL 证书步骤如下：  

1. 安装 **openssl** 工具：  
   ```bash
   sudo yum install openssl -y
   ```
2. 创建存放证书的目录：  
   ```bash
   mkdir -p /etc/ssl
   ```
3. 生成私钥：  
   ```bash
   openssl genrsa -out /etc/ssl/test.key 2048
   ```
4. 生成证书签名请求 (CSR)：  
   ```bash
   openssl req -new -key /etc/ssl/test.key -out /etc/ssl/test.csr -subj "/C=CN/ST=guangdong/L=shenzhen/O=group/OU=unit/CN=你的ip"
   ```
5. 生成自签名证书：  
   ```bash
   openssl x509 -req -days 365 -in /etc/ssl/test.csr -signkey /etc/ssl/test.key -out /etc/ssl/test.crt
   ```
