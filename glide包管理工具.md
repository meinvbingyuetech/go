```
执行：glide install，提示：[ERROR] $GOPATH is not set.
 
解决:
1. 执行 go env 查询到 GOPATH (e.g: /data/app/go-project)
------
2. cd到src目录的项目目录下 (e.g: /data/app/go-project/src/test)
------
3. 创建glide.yaml
package: .
ignore:
- user.portrait.sscf.com
import:
- package: github.com/jinzhu/gorm
  version: ~1.0.0
- package: github.com/gin-gonic/gin
  version: ~1.2.0
- package: github.com/cihub/seelog
  version: ~2.6.0
------
4. glide install
```
 
- 修改  github.com\Masterminds\glide\path\winbug.go
 
- line  75
```
cmd := exec.Command("cmd.exe", "/c", "xcopy /s/y", o, n+"\\")
```
 
- 重新 go build
 
- 拷贝 exe 文件到bin

----

- 安装
```
go get github.com/Masterminds/glide
```

- 生成配置文件
```
glide init
```

- 测试安装包
```
glide get github.com/mattn/go-adodb
```
