iOS启动优化执行方案 - 二进制重排

# 一、背景

- 背景：目前启动时间

## 优化结果：

机型：iphone Xs max   

版本：14.7.1

App版本：1.0.2

优化前启动时间

```Objective-C
eg: 版本1.0.2

Total pre-main time: 3.1 seconds (100.0%)

     dylib loading time: 998.95 milliseconds (31.7%)

    rebase/binding time: 442.95 milliseconds (14.0%)

      ObjC setup time: 207.83 milliseconds (6.6%)

      initializer time: 1.4 seconds (47.5%)

      slowest intializers :

       libSystem.B.dylib :  9.73 milliseconds (0.3%)

         GPUToolsCore : 69.78 milliseconds (2.2%)

     libglInterpose.dylib : 530.05 milliseconds (16.8%)

         AFNetworking : 145.41 milliseconds (4.6%)

            CPLive : 988.96 milliseconds (31.4%)



5次平均时间：

(3.2+3.1+3.1+3.3+2.9)/5 = 3.12s

Total pre-main time: 3.12 seconds (100.0%)     
```



优化后启动时间

```Objective-C
eg: 版本1.0.4

Total pre-main time: 1.5 seconds (100.0%)

     dylib loading time: 677.38 milliseconds (44.9%)

    rebase/binding time: 148.93 milliseconds (9.8%)

      ObjC setup time: 27.28 milliseconds (1.8%)

      initializer time: 653.68 milliseconds (43.3%)

      slowest intializers :

       libSystem.B.dylib : 12.03 milliseconds (0.7%)

         AFNetworking : 126.13 milliseconds (8.3%)

          FURenderKit : 38.32 milliseconds (2.5%)

            CPLive : 635.31 milliseconds (42.1%)

            

5次平均时间：

(1.6+1.5+1.6+1.5+1.7)/5 = 1.58s

Total pre-main time: 1.58 seconds (100.0%)
```



# 二、操作步骤

## 2.1 检测无用代码 及 删除

## 2.2 二进制重排

1. 二进制重排原理
2. 查看当前  page - deflu 顺序
3. Order file 配置、  mappath配置
4. Clang插装  查看启动调用函数
5. Swift 无法获取 解决方案
6. 三方路无法获取解决方案
7. 根据App 业务需求 调整

### 