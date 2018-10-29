## 必须先安装了以下工具
- yum -y install git
- yum -y install gcc

## 下载
- 去 https://studygolang.com/dl 这里下载
- wget https://studygolang.com/dl/golang/go1.11.linux-amd64.tar.gz
- tar -zxvf go1.11.linux-amd64.tar.gz

## 配置
- vi /etc/profile
```
export GOROOT=/data/go
export PATH=$GOROOT/bin:$PATH
export GOPATH=/data/app/go-project

# /data/go 为你解压后的路径
# /data/app/go-project 为你创建的第一个工作目录 同时这个目录还要创建 bin src pkg 三个目录
```

- source /etc/profile

## 测试
```
$ go version
```
