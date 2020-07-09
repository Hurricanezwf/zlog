# 简介

这是一个预定义的Log库，用于开发项目时快速接入. 面向接口编程.



# 特点

* 面向接口编程，实现上易扩展
* 最基本的实现是基于Golang的log库进行的封装，简单，无特殊依赖.
* 支持通过Context注入额外上下文参数，例如：TraceId, 服务名，模块名等，在使用SLS进行问题定位的时候搜索方便。


# 快速上手

```Go
func Demo() {
    ctx := log.WithTraceId(context.Background(), "696ce42f-a56c-40a7-accf-035671b81ca6")
    logger := log.NewLogFactory().DefaultLogger(log.LevelDebug)
    logger.Info(ctx, "hello guys, this is a info log")
}
```
