# 安装swag cli 及下载相关包

要使用swaggo,首先需要安装swag cli。

```
$ go get -u github.com/swaggo/swag/cmd/swag
```

然后我们还需要两个包。

# gin-swagger 中间件

```
$ go get github.com/swaggo/gin-swagger
```

# swagger 内置文件

```
$ go get github.com/swaggo/gin-swagger/swaggerFiles
```

# 在api处根据api添加描述



```
import (
	"github.com/gin-gonic/gin"
	ginSwagger "github.com/swaggo/gin-swagger"
	"github.com/swaggo/gin-swagger/swaggerFiles"
)


// @title Swagger Example API
// @version 1.0
// @description This is a sample server celler server.
// @termsOfService https://razeen.me

// @contact.name Razeen
// @contact.url https://razeen.me
// @contact.email me@razeen.me

// @license.name Apache 2.0
// @license.url http://www.apache.org/licenses/LICENSE-2.0.html

// @host 127.0.0.1:8080
// @BasePath /api/v1


func main() {

	r := gin.Default()
    store := sessions.NewCookieStore([]byte("secret"))
	r.Use(sessions.Sessions("mysession", store))

	r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))

	v1 := r.Group("/api/v1")
	{
		v1.GET("/hello", HandleHello)
	}

	r.Run(":8080")
}
```



```
 // @Summary 测试SayHello
// @Description 向你说Hello
// @Tags 测试
// @Accept mpfd
// @Produce json
// @Param who query string true "人名"
// @Success 200 {string} json "{"msg": "hello Razeen"}"
// @Failure 400 {string} json "{"msg": "who are you"}"
// @Router /hello [get]
func HandleHello(c *gin.Context) {
	who := c.Query("who")

	if who == "" {
		c.JSON(http.StatusBadRequest, gin.H{"msg": "who are u?"})
		return
	}

	c.JSON(http.StatusOK, gin.H{"msg": "hello " + who})
}
```

保存之后再命令行写

```
swag init
```

成功后在统计目录会生成一个docs目录,里面有三个文件：

- docs.go
- swagger.json
- swagger.yaml

最后`go run main,go` 也成功

原以为在浏览器访问 http://127.0.0.1:8080/swagger/index.html 肯定是成功的，没想到页面显示是 Fetch errorInternal Server Error doc.json

一开始可能是注释写漏了或者是写错了，但是各种尝试修改都不行。
最后查到说是需要引入docs目录才行。



```
import (
	"github.com/gin-gonic/gin"
	ginSwagger "github.com/swaggo/gin-swagger"
	"github.com/swaggo/gin-swagger/swaggerFiles"

        _ "gin-admin/docs"
)
```

最后用上面的方式果然成功