# JPush Api Golang Client

## 快速开始

### 下载安装

```bash
$ go get -u -v github.com/hongbook/jpush-api-go-client
```

### 创建文件 `push.go`

```go
package main

import (
	"context"
	"fmt"

	"github.com/hongbook/jpush-api-go-client"
)

func main() {
	jpush.Init(2,
		jpush.SetAppKey("1111"),
		jpush.SetMasterSecret("2222"),
	)

	defer jpush.Terminate()

	payload := &jpush.Payload{
		Platform:     jpush.NewPlatform().All(),
		Audience:     jpush.NewAudience().All(),
		Notification: jpush.NewNotification().SetAlert("通知测试"),
		Options:      jpush.NewOptions().SetSendNO(1),
	}
	err := jpush.Push(context.Background(), payload, func(result *jpush.PushResult, err error) {
		if err != nil {
			panic(err)
		}
		fmt.Println(result.String())
	})
	if err != nil {
		panic(err)
	}
}

```

### 编译运行

```bash
$ go build push.go
$ ./push
```

> 输出结果
```json
{"sendno":"1","msg_id":"1234567890"}
```

## 特性

- 支持异步推送队列
- 自动处理频率限制
- 自动维护cid池

## MIT License

    Copyright (c) 2020 hongbook

