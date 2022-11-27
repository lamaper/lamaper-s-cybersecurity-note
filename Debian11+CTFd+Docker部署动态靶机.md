# Debian11+CTFd+Docker部署动态靶机

## 一、准备工作

### 换源

#### apt换源

存放apt源的配置文件路径为/etc/apt/source.list，首先要对这个配置文件进行备份，备份命令如下。

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

 如果需要**恢复原来的配置文件**，只需要用备份的配置文件覆盖原来的配置文件即可，命令如下。

```bash
sudo cp /etc/apt/sources.list.bak /etc/apt/sources.list
```

 使用nano打开source.list文件，命令如下。

```bash
sudo nano /etc/apt/sources.list
```

根据需要进行换源，这里更换为清华大学源：

```bash
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch main non-free contrib
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-updates main non-free contrib
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-backports main non-free contrib
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch main non-free contrib
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-updates main non-free contrib
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-backports main non-free contrib
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security/ stretch/updates main non-free contrib
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security/ stretch/updates main non-free contrib
```

#### 安装pip及换源

安装pip

```bash
sudo apt install python3-pip
```

换源

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
```

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

安装flask

```bash
sudo pip install flask
```

安装gunicore（后台运行）

```bash
sudo pip install gunicore
```

安装gevent

```bash
sudo pip intsall gevent
```

#### 安装git

```bash
sudo apt install git
```

#### 安装ssh（可选）

```bash
sudo apt install ssh
```

修改配置文件

### 克隆仓库

下载改写的ctfd，赵师傅已经完成了镜像换源等操作

```bash
sudo git clone https://github.com/glzjin/CTFd.git
```

下载frp

```bash
wget https://github.com/fatedier/frp/releases/download/v0.29.0/frp_0.29.0_linux_amd64.tar.gz

tar -zxvf frp_0.29.0_linux_amd64.tar.gz
```

下载ctf-whale

```bash
sudo git clone https://github.com/glzjin/CTFd-Whale.git
```

下载docker的frps

```bash
sudo git clone https://github.com/glzjin/Frp-Docker-For-CTFd-Whale.git
```



