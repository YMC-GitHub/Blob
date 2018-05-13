less 使用手册
# 目录
- [语法](#语法)
	- [变量](#变量)
		- 定义
		- 使用
			- 用于选择器名
			- 用于属性名字
			- 用于属性取值
			- 用于变量取值
		- 注意
	- [混合](#混合)
		- 概念
		- 示例
		- 带参
		- 带参+默认取值
		- 不带参数
		- 模式匹配
			- 根据传入的参数类型
			- 根据传入的参数数量
		- 引导序列
			- 式子
			- 运算
			- 带参
			- 类型
			- 条件
	- [嵌套](#嵌套)
	- [函数](#函数)
		- 颜色
		- 数学
	- [导入](#导入)
		- 后缀可带
		- 跳过处理
	- [注释](#注释)
- [特效](#特效)
	- [圆角半径](#圆角半径)
	- [方块阴影](#方块阴影)
	- [过渡效果](#过渡效果)
	- [转换变换](#转换变换)	
	- [线性渐变](#线性渐变)
	- [快速渐变](#快速渐变)	
	- [镜像效果](#镜像效果)
	- [互补颜色](#互补颜色)	
	- [颜色微调](#颜色微调)	
- [安装](#安装)
- [编译](#编译)
	- 命令行方式
	- 服务端方式
	- 客户端方式
- [形状](#形状)
	- [正方形](#正方形)
	- [矩形](#矩形)
	- [圆形](#圆形)
	- [椭圆](#椭圆)
	- [三角](#三角)
	- [弯尾箭头](#弯尾箭头)
	- [梯形](#梯形)
	- [平行四边形](#平行四边形)
	- [六角星](#六角星)
	- [五角星](#五角星)
	- [五边形](#五边形)
	- [六边形](#六边形)
	- [八边形](#八边形)
	- [心形](#心形)
	- [菱形](#菱形)
	- [钻石](#钻石)
	- [鸡蛋](#鸡蛋)
	- [聊框](#聊框)
	- [放大镜](#放大镜)
	- [圆锥形](#圆锥形)
	- [月亮](#月亮)
	- [十字架](#十字架)
- [参考](#参考)
# 语法

## 变量

### 定义
```less
// 数字

// 字符

// 颜色

// 单位

// 函数

// 变量

// 式子


@ns: ~"am-";

// color
@black:           #000;
@white:           #fff;

@global-primary:	    #0e90d2; // #157EFB; //#428bca;
@global-secondary:    lighten(@global-primary, 15%);
@global-success:	    #5eb95e; // #53D76A; //#5cb85c;
@global-warning:	    #F37B1D; // #FD9426; // #f0ad4e;
@global-danger:		    #dd514c; // #FC3159; //#da314b;
@global-info:         @global-secondary;

@one-primary: #15afef;

```
### 使用
#### 用于选择器名
#### 用于属性名字
#### 用于属性取值
#### 用于变量名字

### 注意
```
变量是延迟加载的，不一定要在使用之前定义
当变量被多次定义时，以最后一次定义为准，且从当前作用域向上搜寻
```

## 混合
### 概念
定义一些通用的属性集为一个class，然后在另一个class中去调用这些属性。

### 示例
```less
//less文件
.bordered {
	border-top:dotted 1px black;
	border-bottom:solid 2px black;
}
#menu a{
color:#111;
.bordered;
}
```

```css
/*css文件*/
#menu a {
color:#111;
border-top:dotted 1px black;
border-bottom:solid 2px black;
}
```
### 带参

### 带参+默认取值

### 不带参数

### 模式匹配
#### 根据传入的参数类型

#### 根据传入的参数数量

### 导引序列
#### 式子

#### 运算

#### 带参

#### 类型

#### 条件

## 嵌套

## 函数
### 颜色
|名字|示例|描述|
|:----|:----|:----|
|lighten|```lighten(@color,10%);```|亮度更轻|
|darken|```darken(@color,10%);```|亮度更暗|
|saturate|```saturate(@color,10%);```|饱和度更多|
|desaturate|```desaturate(@color,10%);```|饱和度更少|
|fadeout|```fadeout(@color,10%);```|透明度更多|
|fadein|```fadein(@color,10%);```|透明度更少|
|fade|```fade(@color,10%);```|透明度为10%|
|spin|```spin(@color,10%);```|色调更多|
|spin|```spin(@color,-10%);```|色调度更少|
|mix|```mix(@color,@color2);```|混合|
|hue|```hue(@color);```|提取颜色色调|
|saturation|```hue(@color);```|提颜色饱和度|
|lightness|```hue(@color);```|提取颜色亮度|
|hsl|```:hsl(hue(@old),45%,90%);```|创建hsl颜色|

### 数学
|名字|示例|描述|
|:----|:----|:----|
|round|```round(1.67);```|四舍五入|
|ceil|```ceil(2.4);```|往上取整|
|floor|```floor(2.6);```|往下取整|
|percentage|```percentage(0.5);```|变百分比|

## 导入
### 后缀可带
```less
//方式1
@import"lib.less";
//方式2
@import"lib";
```

### 跳过处理
想导入一个CSS文件而且不想less对它进行处理，只需要使用.css后缀就可以
```
@import"lib.css";
```

## 编译
### 跳过编译
```
filter:~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
```

## 注释
```javascript
//方式1:(在css中可见)
/*我是注释内容，在css中可见*/

//方式2:(css中不可见)
//我是注释内容，css中不可见
```


		
# 特效
## 圆角半径
```less
.border-radius((@topleft:5px; @topright:5px; @bottomleft:
5px; @bottomright:5px)){
  border-radius:@topleft @topright @bottomright @bottomleft;
}
```

## 方块阴影
```less
.box-shadow(@x:0px; @y:3px; @blur:5px; @alpha:0.5){
  box-shadow:@x @y @blur rgba(0,0,0,@alpha);
}
```
## 过渡效果
```less
.transition(@prop:all; @time:1s; @ease:linear){
  transition:@prop @time @ease;
}
```

## 转换变换
```less
.transform(@rotate:90deg; @scale:1; @skew:1deg; @translate:
10px){
  transform:rotate(@rotate) scale(@scale) skew(@skew) translate(@translate);
}
```

## 线性渐变
```less
.gradient(@origin:left; @start:#ffffff; @stop:#000000){
  background-color:@start;
  background-image:linear-gradient(@origin,@start,@stop);
}
```

## 快速渐变
```less
.quick-gradient(@origin:left;@alpha:0.2){
  background-image:linear-gradient(@origin,rgba(0,0,0,0.0),rgba(0,0,0,@alpha));

}
```

## 镜像效果
```less
.reflect(@length:50%;@opacity:0.2){
  -webkit-box-reflect:below 0px -webkit-gradient(linear,left top,left bottom,from(transparent),color-stop(@length,
transparent),to(rgba(255,255,255,@opacity)));
}
```

## 互补颜色
```less
@base:#663333;
@complement1:spin(@base,180);
@complement2:darken(spin(@base,180),5%);
@lighten1:lighten(@base,15%);
@lighten2:lighten(@base,30%);
```

## 颜色微调
```less
@base:#663333;
@lighter1:lighten(spin(@base,5),10%);
@lighter2:lighten(spin(@base,10),20%);
@darker1:darken(spin(@base,-5),10%);
@darker2:darken(spin(@base,-10),20%);
```

# 安装

## npm

### 方式

|方式|示例|
|:----|:----|
|全局|```npm install less -g```|
|本地|```npm install less```|
|全局|```npm i less -g```|
|本地|```npm i less```|

### 版本

|版本|示例|
|:----|:----|
|最新|```npm i less@2.7.1```|
|指定|```npm i less@2.7.1```|

### 环境

|版本|示例|
|:----|:----|
|开发时依赖|```npm i less@2.7.1 --save-dev```|
|生产时依赖|```npm i less@2.7.1 --save```|


# 编译
## 命令行方式
### 语法
```
lessc bootstrap.less bootstrap.css
```

### 选项

|选项|简写|说明|描述|
|:----|:----|:----|:----|
|--version|-v|基础-查看版本|Prints version number and exit|
|--help|-h|基础-查看帮助|Prints a help message with available options and exits|
|--depends|-M|Outputs a makefile import dependency list to stdout|
|--include-path=PATHS|——|编译-文件路径|Sets include paths|
|--insecure|——|编译-远程引入|Allows imports from insecure https hosts|
|--url-args='QUERYSTRING'|——|编译-远程引入|Adds params into url tokens|
|--no-ie-compat|——|编译-兼容检查|Disables IE compatibility checks|
|--no-js|——|编译-禁用脚本|Disables JavaScript in less files|
|--strict-units=on|-su=on|编译-混合单位|Allows mixed units|
|--strict-math=on|-sm=on|编译-数学括号|Turns on strict math, where in strict mode, math Requires brackets|
|--global-var='VAR=VALUE'|——|编译-定义变量|Defines a variable that can be referenced by the file|
|--modify-var='VAR=VALUE'|——|编译-修改变量|Modifies a variable already declared in the file|
|--silent|-s|调试-关闭警告|Stops any warnings from being shown|
|--no-color|——|调试-关闭颜色|Disables colorized output|
|--source-map[=FILENAME]|——|调试-开启映射|output filename.map|
|--source-map-url=URL|——|调试-映射路径|Sets a custom URL to map file|
|--source-map-rootpath=X|——|调试-映射路径|Adds this path onto the sourcemap filename and less file paths|
|--source-map-basepath=X|——|调试-映射路径|Sets sourcemap base path|
|--relative-urls|-ru|调试-映射路径|Re-writes relative urls to the base less file|
|--rootpath=URL|-rp|调试-映射路径|Sets sourcemap base path|
|--source-map-less-inline|——|调试-映射内容|Puts the less files into the map instead of referencing them|
|--source-map-map-inline|——|调试-输到样式|Puts the map (and any less files) as a base64 data uri into the output css file|
|--line-numbers=TYPE |——|调试-输出行号|Outputs filename and line numbers|
|--lint|-l|校验-代码校验|Syntax check only (lint)|
|--strict-imports|——| Forces evaluation of imports|
|--verbose|——|Be verbose|
|--compress|-x|压缩-移除空格|Compresses output by removing some whitespaces|
|--plugin=PLUGIN=OPTIONS|——|压缩-使用插件| Loads a plugin|

# 形状
## 正方形
```less
.shape-square(@sp-sq-width:100px;@sp-sq-height:100px;@sp-sq-color:red) {
    width: @sp-sq-width;
    height: @sp-sq-height;
    background: @sp-sq-c;
}
```

## 矩形
```less
.shape-rectangle(@sp-re-width:200px;@sp-re-height:100px;@sp-re-color:red) {
    width: @sp-re-width;
    height: @sp-re-height;
    background: @sp-re-c;
}
```

## 圆形
```less
.shape-circle(@sp-ci-width:100px;@sp-ci-height:100px;@sp-ci-color:red) {
    width: @sp-ci-width;
    height: @sp-ci-height;
    background: @sp-ci-c;
    border-radius: 50%;
}
```

## 椭圆
```less
.shape-oval(@sp-ov-width:200px;@sp-ov-height:100px;@sp-ov-color:red) {
    width: @sp-ov-width;
    height: @sp-ov-height;
    background: @sp-ov-c;
    border-radius: @sp-ov-width/@sp-ov-height;
}
```

## 三角
```less
//向上
.shape-triangle-up(@sp-tu-left:50px;@sp-tu-right:50px;@sp-tu-bottom:100px;@sp-tu-color:red) {
    width: 0;
    height: 0;
		border-left: @sp-tu-left solid transparent;
    border-right: @sp-tu-right solid transparent;
    border-bottom: @sp-tu-bottom solid @sp-tu-color;    
}
//向下
.shape-triangle-down(@sp-td-left:50px;@sp-td-right:50px;@sp-td-top:100px;@sp-td-color:red) {
    width: 0;
    height: 0;
		border-left: @sp-td-left solid transparent;
    border-right: @sp-td-right solid transparent;
    border-top: @sp-td-top solid @sp-td-color;    
}
//向左
.shape-triangle-left(@sp-tl-top:50px;@sp-tl-bottom:50px;@sp-tl-right:100px;@sp-tl-color:red) {
    width: 0;
    height: 0;
    border-top: @sp-tl-top solid transparent;    
		border-bottom: @sp-tl-left solid transparent;
    border-right: @sp-tl-right solid @sp-tl-color;
    
}
//向右
.shape-triangle-right(@sp-tr-top:50px;@sp-tr-bottom:50px;@sp-tr-left:100px;@sp-tr-color:red) {
    width: 0;
    height: 0;
    border-top: @sp-tr-top solid transparent;    
		border-bottom: @sp-tr-left solid transparent;
    border-left: @sp-tr-left solid @sp-tr-color;
}
//左上
.shape-triangle-topleft(@sp-ttl-top:100px;@sp-ttl-right:100px;@sp-ttl-color:red) {
    width: 0;
    height: 0;
    border-top: @sp-ttl-top solid @sp-ttl-color;    
		border-right: @sp-ttl-right solid transparent;
}
//右上
.shape-triangle-topright(@sp-ttr-top:100px;@sp-ttr-left:100px;@sp-ttr-color:red) {
    width: 0;
    height: 0;
    border-top: @sp-ttr-top solid @sp-ttr-color;    
		border-left: @sp-ttr-left solid transparent;
}
//左下
.shape-triangle-bottomleft(@sp-tbl-bottom:100px;@sp-tbl-right:100px;@sp-tbl-color:red) {
    width: 0;
    height: 0;
    border-bottom: @sp-tbl-bottom solid @sp-tbl-color;    
		border-right: @sp-tbl-right solid transparent;
}
//右下
.shape-triangle-bottomright(@sp-tbr-bottom:100px;@sp-tbr-left:100px;@sp-tbr-color:red) {
    width: 0;
    height: 0;
    border-bottom: @sp-tbr-bottom solid @sp-tbr-color;    
		border-left: @sp-tbr-right solid transparent;
}
```

## 弯尾箭头
## 梯形
## 平行四边形
## 六角星
## 五角星
## 五边形
## 六边形
## 八边形
## 心形
## 菱形
## 钻石
## 鸡蛋
## 聊框
## 放大镜
## 圆锥形
## 月亮
## 十字架
```less
.shape-cross(@sp-cr-width:20px;@sp-cr-height:100px;@sp-cr-color:red){
  background: @sp-cr-color;
  height: @sp-cr-height;
  position: relative;
  width: @sp-cr-width;

  &:after {
  background: @sp-cr-color;
  content: "";
  height: @sp-cr-width;
  left: -(@sp-cr-height-@sp-cr-width)/2;
  position: absolute;
  top: (@sp-cr-height-@sp-cr-width)/2;
  width: @sp-cr-height;
  }
}
```
# 参考
