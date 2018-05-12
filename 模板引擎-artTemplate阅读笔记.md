# artTemplate-3.0

新一代 javascript 模板引擎

##	目录

*	[开发手册](#开发手册)
*	[使用手册](#使用手册)
*	[模板语法](#模板语法)

## 开发手册

### 模板编译
```js
/**
 * 编译模板
 * @name    template.compile
 * @param   {String}    模板内容
 * @param   {Object}    编译选项
 *
 *      - openTag       {String} 开始标签
 *      - closeTag      {String} 结束标签
 *      - filename      {String} 文件名字
 *      - escape        {Boolean} 是否转码
 *      - compress      {Boolean} 是否压缩
 *      - debug         {Boolean} 是否调试
 *      - cache         {Boolean} 是否存储
 *      - parser        {Function} 解析函数
 *
 * @return  {Function}  渲染方法
 */
```

### 模板配置
默认配置以及设置配置。
```js
var defaults = template.defaults = {
    openTag: '<%',    // 逻辑语法开始标签
    closeTag: '%>',   // 逻辑语法结束标签
    escape: true,     // 是否编码输出变量的 HTML 字符
    cache: true,      // 是否开启缓存（依赖 options 的 filename 字段）
    compress: false,  // 是否压缩输出
    parser: null      // 自定义语法格式器 @see: template-syntax.js
};
template.config = function (name, value) {
    defaults[name] = value;
};
```

### 模板工具
```js

```

### 模板辅助
```js
template.helper = function (name, helper) {
    helpers[name] = helper;
};
var helpers = template.helpers = utils.$helpers;
```

### 编译缓存
```js
var cacheStore = template.cache = {};
```


### 获取编译
```js
template.get = function (filename) {
    var cache;
    if (cacheStore[filename]) {
        // 用内存中的缓存
        cache = cacheStore[filename];
    } else if (typeof document === 'object') {
        // 加载模板并编译
        var elem = document.getElementById(filename);
        
        if (elem) {
            var source = (elem.value || elem.innerHTML)
            .replace(/^\s*|\s*$/g, '');
            cache = compile(source, {
                filename: filename
            });
        }
    }
    return cache;
};
```


### 渲染模板
```js
template.render = function (source, options) {
    return compile(source, options);
};
var renderFile = template.renderFile = function (filename, data) {
    var fn = template.get(filename) || showDebugInfo({
        filename: filename,
        name: 'Render Error',
        message: 'Template not found'
    });
    return data ? fn(data) : fn;
};
```


### 接口合并
```js
//情景-编译模板:template('test', '{{value}}')
//情景-渲染模板:template('test', data)
var template = function (filename, content) {
    return typeof content === 'string'
    ?
    //编译模板
    compile(content, {
            filename: filename
        })
    :
    //渲染模板
    renderFile(filename, content);
};
//版本
template.version = '3.0.0';
```

## 使用手册

### 编写模板

使用一个``type="text/html"``的``script``标签存放模板：
```html
	<script id="test" type="text/html">
	<h1>{{title}}</h1>
	<ul>
	    {{each list as value i}}
	        <li>索引 {{i + 1}} ：{{value}}</li>
	    {{/each}}
	</ul>
	</script>
```

### 渲染模板
```javascript
	var data = {
		title: '标签',
		list: ['文艺', '博客', '摄影', '电影', '民谣', '旅行', '吉他']
	};
	var html = template('test', data);
	document.getElementById('content').innerHTML = html;
```
# reference
[art-template-offical-tutorial](https://aui.github.io/art-template/zh-cn/docs/index.html)
