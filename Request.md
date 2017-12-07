```go
// 上一个页面
	referer := c.Request.Referer()
	fmt.Println(referer)
	// 主机
	host := c.Request.Host
	fmt.Println(host)
	// 请求协议
	proto := c.Request.Proto
	fmt.Println(proto)

	// scheme
	scheme := "http://"
	if c.Request.TLS != nil {
		scheme = "https://"
	}

	// 请求完整链接
	fullUrl := strings.Join([]string{scheme, c.Request.Host, c.Request.RequestURI}, "")
	fmt.Println(fullUrl)

	// 请求IP
	ip := strings.Split(c.Request.RemoteAddr, ":")[0]
	fmt.Println(ip)
	return

	ua := c.Request.Header.Get("User-Agent")
	cookie := c.Request.Header.Get("Cookie")
	connection := c.Request.Header.Get("Connection")
	ae := c.Request.Header.Get("Accept-Encoding")
	fmt.Println(ua)
	fmt.Println(cookie)
	fmt.Println(connection)
	fmt.Println(ae)
```
