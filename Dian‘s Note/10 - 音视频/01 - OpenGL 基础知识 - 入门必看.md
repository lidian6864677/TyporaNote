# OpenGL 基础知识 - 入门必看



[TOC]



## 基本概念

## 矩阵模式：

## 投影：	

## [着色器](https://www.tw.wiiaa.top/baike-%E7%9D%80%E8%89%B2%E5%99%A8)

函数、方法代码段  cup来使用  shader代码段-> gpu

- 固定着色器：存储着色器  苹果提供API - 调用 - OpenGL 提供
- 自定义着色器：进行自定义的着色器  GLSL语法进行编写
- Vertex Shader顶点着色器：用来处理顶点相关代码，（三角形） 一个正方形有6个顶点 1
  1. 确定位置   2. 缩放/平移/旋转 位置换算  3.手机端显示3D 手机屏幕实际是2D的  3D图形数据 - 2D 投影换算 - OpenGL ES 
- Fragment Shader片元着色器：片元 （像素点）着色器   处理一个一个的像素点  120*120 像素点 = 14400次 GPU进行运算

```c++
enum GLT_STOCK_SHADER { 
    GLT_SHADER_IDENTITY = 0,             // 单位着色器/单元着色器
    GLT_SHADER_FLAT,                     // 平面着色器
		GLT_SHADER_SHADED,								   // 上色着色器
    GLT_SHADER_DEFAULT_LIGHT,					   // 默认光源着色器
    GLT_SHADER_POINT_LIGHT_DIFF,				 // 点光源着色器
    GLT_SHADER_TEXTURE_REPLACE, 				 // 纹理替换着色器
    GLT_SHADER_TEXTURE_MODULATE,				 // 纹理调整着色器
    GLT_SHADER_TEXTURE_POINT_LIGHT_DIFF, // 纹理光源着色器
    GLT_SHADER_TEXTURE_RECT_REPLACE,		 // 纹理矩形替换着色器
    GLT_SHADER_LAST 										 // 上次使用的着色器
};	
```

### 参数&解释

#### GLT_SHADER_IDENTITY：单位着色器/单元着色器

```c++
/*
     参数1：GLT_SHADER_IDENTITY（着色器标签）
     参数2：颜色值
     */
shaderManager.UseStockShader(GLT_SHADER_IDENTITY,vColor);
```

#### GLT_SHADER_FLAT：平面着色器

```c++

```

#### GLT_SHADER_SHADED：上色着色器

```c++

```



#### GLT_SHADER_DEFAULT_LIGHT：默认光源着色器

```c++

```



#### GLT_SHADER_POINT_LIGHT_DIFF：点光源着色器

```c++

```



#### GLT_SHADER_TEXTURE_REPLACE：纹理替换着色器

```c++

```



#### GLT_SHADER_TEXTURE_MODULATE：纹理调整着色器

```c++

```



#### GLT_SHADER_TEXTURE_POINT_LIGHT_DIFF：纹理光源着色器

```c++

```



#### GLT_SHADER_TEXTURE_RECT_REPLACE：纹理矩形替换着色器

```c++

```

#### GLT_SHADER_LAST：上次使用的着色器

```c++

```

## GLSL

## OpenGL扩展机制：

## 数据类型：

## 错误处理：

## 缓冲区：

## 基本图元：

![image-20210513165459676](https://tva1.sinaimg.cn/large/008i3skNly1gqgwib6ufgj30hz0beabv.jpg)

### 点

-  GL_POINT：在屏幕上就是一个点

### 线段

- GL_LINES： 两个顶点之间连接的线段
- GL_LINE_STRIP：从第一个到最后一个 每一个顶点相连形成的线条
- GL_LINE_LOOP：每一个点相连形成的线条  最后一个点与第一个点相连形成一个闭环

### 多边形

- GL_TRIANGLES：每3个顶点形成一个三角形
- GL_TRIANGLES_STRIP： 每三个点组成一个三角形 存在复用一个（strip）条带，形成一组三角形
- GL_TRIANGLES_FAN：以第一个点为中心 依次链接其它的点，会复用相邻的订单 形成一组三角形

### 



