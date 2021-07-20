# Flutter项目前期准备

### FlutterSDK下载

- git下载SDK

  - git clone -b beta https://github.com/flutter/flutter.git

    -  git clone -b master https://github.com/flutter/flutter.git

  - 如果长时间没下载下来或者连接失败 先设置一下这两个环境变量

    ``` c
    set PUB_HOSTED_URL=https://pub.flutter-io.cn
    set FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
    ```

  - 如果还是不能下载 使用码云的下载链接

  - git clone https://gitee.com/mirrors/Flutter.git

- 添加Flutter配置

  - 下载完成后执行命令 打开 配置文件

  - ```c
    open ~/.bash_profile 
    ```

  - 添加一下配置到此文件中  

  - ```c
    export PUB_HOSTED_URL=https://pub.flutter-io.cn
    export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
    export PATH=你安装Flutter路径/flutter/bin:$PATH
    ```

    - 注意不要有空格！！！

  - 保存退出执行更新配置命令

    ```c
    source ~/.bash_profile
    ```

- 安装Flutter依赖

  - 检测安装Flutter状态

    ```c
    flutter doctor
    ```

  - 根据提示 逐个解决相应问题

  - 第一次运行时比较慢 因为他会下载自己的依赖并编译，之后就快了

  - 一般错误为xcode或者Android Studio版本太低，VSCode导入Flutter包

  - 大部分问题解决方式在文档中都能查到： https://flutterchina.club/setup-macos/

- 配置编译器
  
  - Xcode安装 
  - Android Studio.安装 并配置
    - 启动Android Studio.
    - 打开插件首选项 (
      - macOS：
        - **Preferences>Plugins** 
      - Windows & Linux：
        -  **File>Settings>Plugins** 
    - 选择 **Browse repositories…**, 选择 Flutter 插件并点击 `install`.
    - 重启Android Studio后插件生效.
  - VS Code
    - 启动 VS Code
    - 调用 **View>Command Palette…**
    - 输入 ‘install’, 然后选择 **Extensions: Install Extension** action
    - 在搜索框输入 `flutter` , 在搜索结果列表中选择 ‘Flutter’, 然后点击 **Install**
    - 选择 ‘OK’ 重新启动 VS Code

