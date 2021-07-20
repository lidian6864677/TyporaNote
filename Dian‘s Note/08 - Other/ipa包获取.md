要获得线上APP的ipa文件，现在有以下几种方案
 `1.通过PP助手下载安装到手机的应用`
 `2.通过iTools助手下载安装到手机的应用`
 `3.通过Apple Configurator 2（Mac商店）获取`
 前两种方案网上的教程很多，这里只介绍第三种方案

- 首先 去Mac上的App Store下载Apple Configurator 2。

- 然后把iphone连接上Mac，点击Apple Configurator 2 菜单中->账户->登陆（用连接设备的Apple ID）

  ![img](https:////upload-images.jianshu.io/upload_images/2117012-480040753ee55fb0.png?imageMogr2/auto-orient/strip|imageView2/2/w/728/format/webp)

  打开登录.png

![img](https:////upload-images.jianshu.io/upload_images/2117012-87135922d4a2aa44.png?imageMogr2/auto-orient/strip|imageView2/2/w/591/format/webp)

登录Apple ID.png

- 备份iPhone的内容（避免数据丢失，非必选）

- 所有设备->选中当前iPhone->添加->应用，找到您想要ipa的那个应用->添加

  ![img](https:////upload-images.jianshu.io/upload_images/2117012-b42cb1fa5389bc30.png?imageMogr2/auto-orient/strip|imageView2/2/w/941/format/webp)

  添加应用.png

![img](https:////upload-images.jianshu.io/upload_images/2117012-6b503c88d51a097f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1144/format/webp)

添加.png

![img](https:////upload-images.jianshu.io/upload_images/2117012-fc7e0821627874cb.png?imageMogr2/auto-orient/strip|imageView2/2/w/666/format/webp)

下载ipa包中.png

- 因为你手机中已经存在了当前应用，所以会提示，该应用已经存在， 是否需要替换？
   此时，不要点任何按钮！不要点任何按钮！不要点任何按钮！

  ![img](https:////upload-images.jianshu.io/upload_images/2117012-3b34d1d943d9b4be.png?imageMogr2/auto-orient/strip|imageView2/2/w/449/format/webp)

  不要点击任何按钮.png

- 不要操作Apple Configurator 2，让它保持上图的状态，然后打开Finder前往文件夹，或者直接快捷键`command+shift+G`
   并输入下面路径

  ```
  ~/Library/Group Containers/K36BKF7T3D.group.com.apple.configurator/Library/Caches/Assets/TemporaryItems/MobileApps/
  ```

  

  

  ![img](https:////upload-images.jianshu.io/upload_images/2117012-1891a0b190c8d0f0.png?imageMogr2/auto-orient/strip|imageView2/2/w/445/format/webp)

  前往文件夹.png

  ![img](https:////upload-images.jianshu.io/upload_images/2117012-a5c02b4d75c00cca.png?imageMogr2/auto-orient/strip|imageView2/2/w/429/format/webp)

  前往文件夹2.png

  

点击前往，打开ipa包所在文件。将ipa文件copy出来。



![img](https:////upload-images.jianshu.io/upload_images/2117012-531b2d93adaf3328.png?imageMogr2/auto-orient/strip|imageView2/2/w/939/format/webp)

获取ipa包所在文件夹.png

这时候别忘了点击Apple Configurator 2窗口中的停止，你会发现刚才目录下的文件也消失了

拿到ipa文件后，你可以将后缀`.ipa`改为`.zip`,然后解压

![img](https:////upload-images.jianshu.io/upload_images/2117012-86b56ba6ebf5b687.png?imageMogr2/auto-orient/strip|imageView2/2/w/512/format/webp)

修改文件类型.png


 就可以看到Payload下的包，显示包内容可以看到部分APP的资源以及Assets.car。

![img](https:////upload-images.jianshu.io/upload_images/2117012-1e565e22a8cfcb3f.png?imageMogr2/auto-orient/strip|imageView2/2/w/514/format/webp)

解压.png



如果你要解压Assets.car， 可以使用github上的工具[https://github.com/pcjbird/AssetsExtractor](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fpcjbird%2FAssetsExtractor)

最后，提取出来的资源文件，大家要注意版权，仅供参考，不要直接拿来商业使用。