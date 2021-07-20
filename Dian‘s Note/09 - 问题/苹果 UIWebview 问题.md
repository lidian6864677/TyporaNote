- 跳转到pod是文件下 执行

  ```objc
   find . -type f | grep -e ".a" -e ".framework" | xargs grep -s UIWebView	
  ```

  查看三方库中是否存在UIWebview相应的代码

  然后去 github或者官网去寻找解决办法 

  