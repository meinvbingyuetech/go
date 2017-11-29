修改  github.com\Masterminds\glide\path\winbug.go
 
line  75
cmd := exec.Command("cmd.exe", "/c", "xcopy /s/y", o, n+"\\")
 
重新 go build
 
拷贝 exe 文件到bin
