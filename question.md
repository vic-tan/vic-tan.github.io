---
layout: page title: "question"
header-img: "img/green.jpg"
---

### 问题

## <-----------------------工厂菜单-------------------——>

### DTV 没有声音

> * 1、RMCA_ready 是否正确
> * 2、查看与SVN 上的key 是否相同
> * 3、拿两机子对比下看是否想同
> * 4、找RTK

## <-----------------------串口问题-------------------——>

### 反抄软件升级后启用串口切DTV 无法切到DTV目录下

> * 1、预置DB频道没有id不是1频道，而800，有可以是LCN搜台的，导致id为800。RTK导致无法切到DTV。

### 串口抄失败

> * 1、查看Key 大小是否正确
> * 2、查看mnt/verdor/impdata 是否成功
> * 3、如果是RMCA key 则查看mnt/verdor/impdata/RMCA_ready 与mnt/verdor/tclconfig/pre_key/RMCA 中是否匹配
> * 4、有时接信源烧key有时会NG，拔掉信号线就可以烧录成功了(Attestation key 比较大，会出现这个问题)
> * 5、可以夹版本排查问题

### 遥控测试按“0”键（屏幕有响应）后发串口命令放回值异常

> * 1、是否随机出现还是总是
> * 2、抓日志
> * 3、软件版本路径ID
> * 4、查看日志，中间件调用流程->UartService ->AppUart ->FactoryUart-> startUartThread() - >
    > UartCommunication () -> transferData()->getRemoteCode()

## <-----------------------串口工具-------------------——>

所有串口第一步都要先进入工厂模式
### 白平衡调试

> * 1、全部命令选择——>内置PATTERN 切换
> * 2、执行命令选择——>内置PATTERN 切换
> * 3、串口命令预览——>数据1表示打开白平衡调试，0表示关闭



