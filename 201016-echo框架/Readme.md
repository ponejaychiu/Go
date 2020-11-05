# echo框架

## 一、Win10环境安装：

### 安装echo：

```go
go get -u github.com/labstack/echo/...
```

安装报错如下：

`package golang.org/x/crypto/acme/autocert:......orestablished connection failed because connected host has failed to respond.`

`package golang.org/x/net/http2:...... or established connection failed b                                                                                                                ecause connected host has failed to respond.`

解决方法：

在GitHub上下载[Go](https://github.com/golang)语言的[crypto](https://github.com/golang/crypto)和[net](https://github.com/golang/net)模块并复制到[GoROOT](C:\Go\src\golang.org\x)目录（我的目录C:\Go\src\golang.org\x）下即可，如果没有此目录则手动创建。

### 创建serve.go：

```go
package main

import (
	"net/http"
    
	"github.com/labstack/echo"
)

func main() {
	e := echo.New()
	e.GET("/", func(c echo.Context) error {
		return c.String(http.StatusOK, "Hello, World!")
	})
	e.Logger.Fatal(e.Start(":1323"))
}
```

### 启动服务：

```go
go run server.go
```

浏览器访问[此链接](http://localhost:1323)即可。

