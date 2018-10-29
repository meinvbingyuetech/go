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
export GOPATH=/vagrant/www/third-party-api:/vagrant/www/test
export THIRDPARTYAPISRC=/vagrant/www/third-party-api/src/third.party.api.sscf.com
export TESTSRC=/vagrant/www/test/src/test.sscf.com

# /data/go 为你解压后的路径
# GOPATH 多个项目用:号 ，然后再分别指定各个项目的src目录下的路径
```

- source /etc/profile

## 测试
```
$ go version
```
