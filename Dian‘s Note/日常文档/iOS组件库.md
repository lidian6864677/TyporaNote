iOS 组件库





# 主工程仓库：[可人直播](https://gitlab.coolpi360.com/coolpi_ios/cocolive)

# Specs库地址：

| **所属模块**                                                 | **组件名**                                                   | **版本**   | **功能**                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---------- | ------------------------------------------------------------ |
| 基础模块                                                     | [CPSpecs](https://gitlab.coolpi360.com/coolpi_ios/cpspecs)   | -          | 私有speecs库                                                 |
| [CoolpiToolLib](https://gitlab.coolpi360.com/coolpi_ios/coolpitoollib) | 0.0.3                                                        | 基础工具库 |                                                              |
|                                                              |                                                              |            |                                                              |
| UserModule(用户)                                             |                                                              |            | 用户基础模块 Api                                             |
| hybridApp模块                                                | [ HybridWebview_Extension](https://gitlab.coolpi360.com/coolpi_ios/hybridwebview_extension) | 0.2.0      | hybridApp模块  Api                                           |
| LiveModule(直播)                                             |                                                              |            | 直播模块  Api                                                |
| ChatModule(聊天)                                             |                                                              |            | 聊天模块  Api                                                |
| MallModule(商城)                                             |                                                              |            | 商城模块  Api                                                |
| GiftModule(礼物)                                             |                                                              |            | 礼物模块  Api                                                |
| CPJXSegmentedView                                            | https://gitlab.coolpi360.com/coolpi_ios/cpjxsegmentedview    |            | 由于capacitor与其内部命名冲突问题单独fork词库并修改部分代码（scrollView） |

# 使用流程

- 查看当前 repo list 是否存在 CPSpecs

pod repo list

- 添加repo Specs 

pod repo add CPSpecs https://gitlab.coolpi360.com/coolpi_ios/cpspecs.git

**⚠️** 可能会需要 输入 gitlab账户密码       输入自己的账户密码即可

# CPJXSegmentedView

CPJXSegmentedView



# iOS 组件库创建流程

- 创建lib

pod lib create 仓库名

**⚠️** 可能会涉及到一些问题

```Swift
What platform do you want to use?? [ iOS / macOS ]（应用于那个平台？）

What language do you want to use?? [ Swift / ObjC ] (什么语言)

Would you like to include a demo application with your library? [ Yes / No ](是否添加Demo)

Which testing frameworks will you use? [ Quick / None ]（是否需要测试框架 一般None）

Would you like to do view based testing? [ Yes / No ] （是否需要做一个测试 一般No）
```

以上结束后 会自动打开xcode

- 替换 ReplaceMe.swift文件名

![image-20220516152038151](https://tva1.sinaimg.cn/large/e6c9d24ely1h2a9vb6oloj206n0640sr.jpg)开始编写需要的组件代码

- gitlab创建远程仓库: **[coolpi_iOS组地址](https://gitlab.coolpi360.com/coolpi_ios)**

**⚠️** 无需多余操作直接起名创建就可以了

**⚠️** cd到当前项目中   按照gitlab 中 push an existing Git repository 提示的操作一次。

cd 组件名

![image-20220516152102474](https://tva1.sinaimg.cn/large/e6c9d24ely1h2a9vdy53zj20i5046glp.jpg)

- 添加tag tag要与下方CPGiftNotice.podspec 文件中的版本相同

git tag '0.1.0'    tag要与下方CPGiftNotice.podspec 文件中的版本相同



git push --tags

- 修改仓库数据  
  - 版本号、描述     source 等

![image-20220516152114955](https://tva1.sinaimg.cn/large/e6c9d24ely1h2a9vfwf1gj207d05lmx8.jpg)

```Ruby
#

# Be sure to run `pod lib lint CPGiftNotice.podspec' to ensure this is a

# valid spec before submitting.

#

# Any lines starting with a # are optional, but their use is encouraged

# To learn more about a Podspec see https://guides.cocoapods.org/syntax/podspec.html

#



Pod::Spec.new do |s|

 s.name       = 'CPGiftNotice'

 s.version     = '0.1.0'

 s.summary     = 'CPGiftNoticeLib的'

 s.swift_version  = '5.0'



# This description is used to generate tags and improve search results.

#  * Think: What does it do? Why did you write it? What is the focus?

#  * Try to keep it short, snappy and to the point.

#  * Write the description between the DESC delimiters below.

#  * Finally, don't worry about the indent, CocoaPods strips it!



 s.description   = <<-DESC

TODO: Add long description of the pod here.

            DESC



 s.homepage     = 'https://gitlab.coolpi360.com/coolpi_ios/cpgiftnotice'

 # s.screenshots   = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2'

 s.license     = { :type => 'MIT', :file => 'LICENSE' }

 s.author      = { 'git' => 'lidian@coolpi360.com' }

 s.source      = { :git => 'https://gitlab.coolpi360.com/coolpi_ios/cpgiftnotice.git', :tag => s.version.to_s }

 # s.social_media_url = 'https://twitter.com/<TWITTER_USERNAME>'



 s.ios.deployment_target = '10.0'



 s.source_files = 'CPGiftNotice/Classes/**/*'

  

 # s.resource_bundles = {

 #  'CPGiftNotice' => ['CPGiftNotice/Assets/*.png']

 # }



 # s.public_header_files = 'Pod/Classes/**/*.h'

 # s.frameworks = 'UIKit', 'MapKit'

 # s.dependency 'AFNetworking', '~> 2.3'

end
```

- 本地验证仓库

pod spec lint 

- 推送当前库中 CPGiftNotice.podspec文件 到CPSpecs

pod repo push CPSpecs CPGiftNotice.podspec

Done！！！





# 更新组件

git status

git add .

git commit -m

git push

git tag '0.2.0'    tag要与下方xxx.podspec 文件中的版x本相同

git push --tags

pod spec lint

pod repo push CPSpecs xxx.podspec







# 现有项目上传至gitlab

pod spec create xxx

会生成: xxx.podspec  并修改其内内容。

Next  与更新组件一致







# Android

1. Failed to resolve: com.github.ionic-team:capacitor:3.4.3
2. 



![image-20220516152201233](https://tva1.sinaimg.cn/large/e6c9d24ely1h2a9v7ugkjj210s0i23zo.jpg)