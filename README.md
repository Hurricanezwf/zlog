# 简介

这是一个预定义的Log库，用于开发项目时快速接入. 面向接口编程.



# 特点

* 面向接口编程，实现上易扩展
* 最基本的实现是基于Golang的log库进行的封装，简单，无特殊依赖.
* 支持通过Context注入额外上下文参数，例如：TraceId, 服务名，模块名等，在使用SLS进行问题定位的时候搜索方便。


# 快速上手

```Go
package main

import (
	"context"

	"github.com/Hurricanezwf/zlog"
)

func main() {
	appContext := zlog.WithServiceName(context.Background(), "app-demo")
	logger := zlog.NewLogFactory().DefaultLogger(zlog.LevelDebug)

	requestContext := zlog.WithTraceId(context.Background(), "696ce42f-a56c-40a7-accf-035671b81ca6")
	NewModule(appContext, logger).DoSomething(requestContext)
}

type Module struct {
	ctx    context.Context
	logger zlog.Logger
}

func NewModule(ctx context.Context, logger zlog.Logger) *Module {
	return &Module{
		ctx:    zlog.WithModuleName(ctx, "main.Module"),
		logger: logger,
	}
}

func (m *Module) DoSomething(ctx context.Context) {
	ctx = zlog.CopyContextValue(m.ctx, ctx)
	m.logger.Info(ctx, "start to do something ...")
}


// Example Output:
2020/07/10 08:55:44 main.go:31:  INFO  --- [ app-demo | main.Module | 696ce42f-a56c-40a7-accf-035671b81ca6 ] :   start to do something ...
```
