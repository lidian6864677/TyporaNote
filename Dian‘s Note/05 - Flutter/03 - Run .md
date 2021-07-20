## 

### 运行时会卡在Running 'gradle assembleDebug, 

- 因为Gradle的Maven仓库在国外, 可以使用阿里云的镜像地址。

## 解决办法

修改项目下 build.gradle 和 flutter 安装目录flutter/packages/flutter_tools/gradle/flutter.gradle 两个文件中 buildscript 和allprojects 中的

```undefined
google()
jcenter()
```

改为阿里云镜像

```rust
maven { url 'https://maven.aliyun.com/repository/google' }
maven { url 'https://maven.aliyun.com/repository/jcenter' }
maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
```





【问题分析：】新下载的sdk没有同意Android协议。
【解决方案：】按错误提示所说的那样，执行命令`flutter doctor --android-licenses`，然后出现的提示让你选择 `y/n`，你只要输入`y`，然后回车，一直坚持到最后就好了。