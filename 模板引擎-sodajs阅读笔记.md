
# 从sodajs中学到的知识点

- 模板指令
	- 前缀 存储|定义|修改
	- 指令 存储 添加|移除|获取|修改
		- 地图存储
		- 数组存储
	- 指令注册
- 模板表达式
	- 标识符 前缀后缀 存储|定义|修改
	- 内容 解析
		- parseSodaExpression
		- compileNode
		- nodes2Arr
- 模板事件
	- 存储
	- 监听|移除|触发
- 数据获取

- 模板数据关联

-文档节点操作
	- 元素类名 追加|移除
	- 元素属性值获取(用原生js dom)




## 正则动态生成
``` js
	//类名正则
    var classNameRegExp = function(className) {
        return new RegExp('(^|\\s+)' + className + '(\\s+|$)', 'g');
    };
    //前缀正则
    var prefix = 'soda';
    var prefixReg = new RegExp('^' + prefix + '-');
    //表达式正则
    var valueoutReg = /\{\{([^\}]*)\}\}/g;
    
    //类型正则
    //标识
    var IDENTOR_REG = /[a-zA-Z_\$]+[\w\$]*/g;
    //字符
    var STRING_REG = /"([^"]*)"|'([^']*)'/g;
    //数字
    var NUMBER_REG = /\d+|\d*\.\d+/g;
	//对象
    var OBJECT_REG = /[a-zA-Z_\$]+[\w\$]*(?:\s*\.\s*(?:[a-zA-Z_\$]+[\w\$]*|\d+))*/g;
    var OBJECT_REG_NG = /[a-zA-Z_\$]+[\w\$]*(?:\s*\.\s*(?:[a-zA-Z_\$]+[\w\$]*|\d+))*/;
	//属性正则
    var ATTR_REG = /\[([^\[\]]*)\]/g;
    var ATTR_REG_NG = /\[([^\[\]]*)\]/;
    var ATTR_REG_DOT = /\.([a-zA-Z_\$]+[\w\$]*)/g;

    var NOT_ATTR_REG = /[^\.|]([a-zA-Z_\$]+[\w\$]*)/g;

	//or正则
    var OR_REG = /\|\|/g;
    var OR_REPLACE = "OR_OPERATOR\x1E"; 
   
	//常量正则
	// @规则:常量以"_$C$_"开头
    var CONST_PRIFIX = "_$C$_";
    var CONST_REG = /^_\$C\$_/;
    var CONST_REGG = /_\$C\$_[^\.]+/g;       
```

## 文档节点操作
``` js
	//元素追加类名
    var addClass = function(el, className) {
        if (!el.className) {
            el.className = className;
            return;
        }

        if (el.className.match(classNameRegExp(className))) {} else {
            el.className += " " + className;
        }
    };
    //元素移除类名
    var removeClass = function(el, className) {
        el.className = el.className.replace(classNameRegExp(className), "");
    };
```



## 模板指令前缀
``` js
	//前缀字符存储
    var prefix = 'soda';
    //前缀正则存储
    var prefixReg = new RegExp('^' + prefix + '-');
    //前缀字符修改
	sodaRender.prefix = function(newPrefix) {
		//替换内置指令的前缀
        for (var key in sodaDirectiveMap) {
            if (sodaDirectiveMap.hasOwnProperty(key)) {
            	//添加新的前缀指令
                sodaDirectiveMap[key.replace(prefix, newPrefix)] = sodaDirectiveMap[key];
                //删除旧的前缀指令
                delete sodaDirectiveMap[key];
            }
        }

        var i = 0,
            len = sodaDirectiveArr.length;
        for (; i < len; i++) {
            sodaDirectiveArr[i].name = sodaDirectiveArr[i].name.replace(prefix, newPrefix);
        }
		
		//前缀字符修改和正则修改
        prefix = newPrefix;
        prefixReg = new RegExp('^' + prefix + '-')
    }    
    
```
指令前缀的存储，以原始字符和指令正则的方式保存，修改的时候需要2个都修改。
另外，soda内置一些指令，修改指令前缀时需要更新内置指令的前缀（若内置的指令前缀不改，会不会有问题呢？下面再讲到）。
## 模板指令存储
sodajs模板引擎使用对象sodaDirectiveMap和数组sodaDirectiveArr存储指令。
``` js
	//存储
    var sodaDirectiveMap = {};
    var sodaDirectiveArr = [];
```
## 模板指令注册
sodajs模板引擎注册指令时，输入指令的名字，以及一个不带参数的回调函数。
引擎会给指令名字加上前缀，以及执行回调函数并用sodaDirectiveMap对象和sodaDirectiveArr数组存储。
回调函数返回的是一个对象，带有priority属性(表示权重)以及一个link函数(处理数据和模板)。
``` js
	//指令注册函数
    var sodaDirective = function(name, func) {
        var name = prefix + '-' + name;
        sodaDirectiveMap[name] = func();

        sodaDirectiveArr.push({
            name: name,
            opt: sodaDirectiveMap[name]
        });
    };
    //指令注册实例:if指令
    sodaDirective('if', function() {
        return {
            priority: 9,
            link: function(scope, el, attrs) {
            	//获取dom中带有if前缀的属性值
                var opt = el.getAttribute(prefix + '-if');

                var expressFunc = parseSodaExpression(opt, scope);

                if (expressFunc) {} else {
                    el.parentNode && el.parentNode.removeChild(el);
                }
            }
        };
    });
```
## 模板指令使用
sodajs模板使用指令时，这么使用：
```html
	<div soda-if="foo">
	</div>
```
## 模板过滤器
```js
	//存储
    var sodaFilterMap = {};
    //添加
    var sodaFilter = function(name, func) {
        sodaFilterMap[name] = func;
    };
    //移除（没有）
    //获取
    sodaFilter.get = function(name) {
        return sodaFilterMap[name];
    };
    //触发 
```









## 数据解析获取
看过js的命名空间书写：
``` js
    var _globals = global;//{a: {b: 1}}
    function namespace(ns) {
        var a = ns.split("."),
            p = _globals, i;
        for (i = 0; i < a.length; i++) {
        	//如果没有，则返回{};而sodajs 没有则返回""
            p[a[i]] = p = p[a[i]] || {};
        }
        return p;
    }
    // eg:
    // _globals = {a: {b: 1}};
    // namespace(""a.b");=>1
```
sodajs的是:getValue
``` js
	//getValue({a: {b: 1}}, "a.b");  // ===>   1
 var getValue = function(_data, _attrStr) {
        
        // 支持常量
        CONST_REGG.lastIndex = 0;
        var realAttrStr = _attrStr.replace(CONST_REGG, function(r) {
            // case:getValue({a: {b: 1}}, "$C$g")
            if (typeof _data[r] === "undefined") {
                return r;
            }
            // case:getValue({a: {b: 1},$C$g:"a"}, "$C$g")
            else {
                return _data[r];
            }
        });
        //case:getValue({a: {b: 1}}, "$C$g") ==> realAttrStr = "$C$g"
        //case:getValue({a: {b: 1},$C$g:"a"}, "$C$g") ==> realAttrStr = "a"

        //case:getValue({a: {b: 1}}, "true")
        if (_attrStr === 'true') {
            return true;
        }
        //case:getValue({a: {b: 1}}, "false")
        if (_attrStr === 'false') {
            return false;
        }

        var _getValue = function(data, attrStr) {
            var dotIndex = attrStr.indexOf(".");
            
            //case:getValue({a: {b: 1}}, "a.b")
            if (dotIndex > -1) {
                var attr = attrStr.substr(0, dotIndex);
                attrStr = attrStr.substr(dotIndex + 1);

                // case:getValue({$C$cc:'a',a: {b: 1}}, "$C$cc.b")
                if (typeof _data[attr] !== "undefined" && CONST_REG.test(attr)) {
                    attr = _data[attr];
                }

				// case:getValue({$C$cc:'a',a: {b: 1}}, "$C$cc.b")
                if (typeof data[attr] !== "undefined") {
                    return _getValue(data[attr], attrStr);
                }
                // case:getValue({$C$cc:'g',a: {b: 1}}, "$C$cc.b")
                else {
                    var eventData = {
                        name: realAttrStr,
                        data: _data
                    };

                    triggerEvent("nullvalue", {
                        type: "nullattr",
                        data: eventData
                    }, eventData);

                    // 如果还有
                    return "";
                }
            } else {

                // 检查attrStr是否属于变量并转换
                if (typeof _data[attrStr] !== "undefined" && CONST_REG.test(attrStr)) {
                    attrStr = _data[attrStr];
                }

                var rValue;
                if (typeof data[attrStr] !== "undefined") {
                    rValue = data[attrStr];
                } else {
                    var eventData = {
                        name: realAttrStr,
                        data: _data
                    };

                    triggerEvent("nullvalue", {
                        type: "nullvalue",
                        data: eventData
                    }, eventData);

                    rValue = attrStr;
                }

                return rValue;
            }
        };

        return _getValue(_data, _attrStr);
    };		
```
getValue(_data, _attrStr)有2个参数，_data是数据对象，_attrStr是数据的字符串表达式。
如：getValue({a: {b: 1}}, "a.b")。

## 模板字符解析
对过滤器进行处理，将模板内容中的表达式与过滤器分开。

``` js
    var parseSodaExpression = function(str, scope) {
		// eg:str = ‘{{item.title|shortTitle:10}}‘
        // 对filter进行处理
        
        str = str.replace(OR_REG, OR_REPLACE).split("|");
        for (var i = 0; i < str.length; i++) {
            str[i] = (str[i].replace(new RegExp(OR_REPLACE, 'g'), "||") || '').trim();
        }

		// eg:expr = "item.title"
        var expr = str[0] || "";
		// eg:filters = ["shortTitle:10"]
        var filters = str.slice(1);

        // 将字符常量保存下来
        expr = expr.replace(STRING_REG, function(r, $1, $2) {
            var key = getRandom();
            scope[key] = $1 || $2;
            return key;
        });

        while (ATTR_REG_NG.test(expr)) {
            ATTR_REG.lastIndex = 0;

            //对expr预处理
            expr = expr.replace(ATTR_REG, function(r, $1) {
                var key = getAttrVarKey();
                // 属性名称为字符常量
                var attrName = parseSodaExpression($1, scope);

                // 给一个特殊的前缀 表示是属性变量
                scope[key] = attrName;
                return "." + key;
            });
        }

        expr = expr.replace(OBJECT_REG, function(value) {
            return "getValue(scope,'" + value.trim() + "')";
        });


        var parseFilter = function() {
			// eg:filters = ["shortTitle:10"]
			//note:arr.shilf del the fist val of filters arr and return the val.
            var filterExpr = filters.shift();

            if (!filterExpr) {
                return;
            }

			// eg:filterExpr = ["shortTitle","10"]			
            var filterExpr = filterExpr.split(":");
			// eg:args = ["10"]
            var args = filterExpr.slice(1) || [];
			// eg:name = ["shortTitle"]
            var name = filterExpr[0] || "";


			// @功能:生成字符正则表达式
			// @规则:以’"‘或’'‘开头,以任意字符中间,以’"‘或’'‘开头结尾
            var stringReg = /^'.*'$|^".*"$/;
            for (var i = 0; i < args.length; i++) {
                //这里根据类型进行判断
                if (OBJECT_REG_NG.test(args[i])) {
                    args[i] = "getValue(scope,'" + args[i].replace(/^'(.*)'$|^"(.*)"$/g, "$1") + "')";
                } else {}
            }

            if (sodaFilterMap[name]) {
				// eg:args = ["item.title","10"]			
                args.unshift(expr);

				// eg:args = "item.title,10"
                args = args.join(",");

				// eg:expr = "sodaFilterMap['shortTitle'](item.title,10)"
                expr = "sodaFilterMap['" + name + "'](" + args + ")";
            }

            parseFilter();
        };
        parseFilter();

		// Function(arg1,...,code)
		// eg:evalFunc = function sodaExp(scope){ return sodaFilterMap['shortTitle'](item.title,10) })
        var evalFunc = new Function("getValue", "sodaFilterMap", "return function sodaExp(scope){ return " + expr + "}")(getValue, sodaFilterMap);
        return evalFunc(scope);
    };
	
```

## 模板数据关联
### 入口：sodaRender(str, data)
其中，str是模板，data是模板需要用到的数据。
1、创建元素，并把元素追加到body标签中(避免自定义标签不生效;追加前，设置元素不显示)，设置元素内容为模板字符源码；
```js
      	var div = document.createElement("div");
        if (document.documentMode < 9) {
            div.style.display = 'none';
            document.body.appendChild(div);
        }
        div.innerHTML = str;
```
2、将元素子节点转换为数组，然后遍历节点数组并编译。
```js
        nodes2Arr(div.childNodes).map(function(child) {
            compileNode(child, data);
        });
```
元素节点转换为节点数组node2Arr的实现：
```js
    var nodes2Arr = function(nodes) {
        var arr = [];

        for (var i = 0; i < nodes.length; i++) {
            arr.push(nodes[i]);
        }

        return arr;
    };
```
编译节点的compileNode是怎么实现的呢？后面再说。
3、获取编译后的元素内容，body标签删除元素并返回元素内容。
```js
        var innerHTML = div.innerHTML;
        if (document.documentMode < 9) {
            document.body.removeChild(div);
        }
        return innerHTML;
```

### 编译：
compileNode(child, data)带有2个参数，child为模板(元素节点)，第二个参数为数据。
元素节点可分为文本节点，属性节点和(孩)子节点。某一元素节点可能同时带有这三种节点。
编译文本节点:
```js
        if (node.nodeType === 3) {
            node.nodeValue = node.nodeValue.replace(valueoutReg, function(item, $1) {
                var value = parseSodaExpression($1, scope);
                if (typeof value === "object") {
                    value = JSON.stringify(value, null, 2)
                }
                //返回处理过后的表达式取值
                return value;
            });
        }
```
如果元素节点是文本节点，获取元素内容；查找模板表达式的内容，解析，然后返回解析后的内容，将解析后的内容替换模板原先的内容；将替换后的内容赋值给元素。
编译属性节点:
sodajs在属性上添加了指令功能，扩展了html的属性。
```js
        if (node.attributes) {
            // 指令处理
            sodaDirectiveArr.map(function(item) {
                var name = item.name;
                var opt = item.opt;
                if (node.getAttribute(name) && node.parentNode) {
                    opt.link(scope, node, node.attributes);
                }
            });

            // 处理输出 包含 prefix-*
            [].map.call(node.attributes, function(attr) {
                // 如果dirctiveMap有的就跳过不再处理（前面已处理）。
                if (!sodaDirectiveMap[attr.name]) {
                    if (prefixReg.test(attr.name)) {
                        var attrName = attr.name.replace(prefixReg, '');

                        if (attrName) {
                            if (attr.value) {
                                var attrValue = attr.value.replace(valueoutReg, function(item, $1) {
                                    return parseSodaExpression($1, scope);
                                });

                                node.setAttribute(attrName, attrValue);
                            }
                        }

                        // 对其他属性里含expr 处理
                    } else {
                        if (attr.value) {
                            attr.value = attr.value.replace(valueoutReg, function(item, $1) {
                                return parseSodaExpression($1, scope);
                            });
                        }
                    }
                }
            });

        }
```
sodajs在html属性上的扩展有三种。一种是指令，一种是指令以外包含prefix-*前缀，还有一种是不带有前缀，但属性取值含有模板表达式。
第一种：
如果是属性节点，遍历存储指令的数组，获取指令的名字和指令的处理函数，执行指令处理函数。
第二种：指令以外包含prefix-*前缀
如果是属性节点，也不是指令，prefix-*前缀，在属性取值中查找模板表达式的内容，解析，然后返回解析后的内容，将解析后的内容替换模板原先的内容；将替换后的内容赋值给元素属性取值。
第三种：不带有前缀，但属性取值含有模板表达式
在属性取值中查找模板表达式的内容，解析，然后返回解析后的内容，将解析后的内容替换模板原先的内容；将替换后的内容赋值给元素属性取值。

编译孩子节点:
```js
        nodes2Arr(node.childNodes).map(function(child) {
            compileNode(child, scope);
        });
```
### 解析：

- 文本节点操作(解析)
    - 文本节点类型判断 node.nodeType === 3
	- 文本节点内容获取  node.nodeValue
	- 文本节点内容修改  node.nodeValue
- 属性节点操作(解析)
	- 属性节点类型判断 node.attributes
	- 属性节点取值获取 node.getAttribute(name)
	- 属性节点取值赋值 node.setAttribute(attrName, attrValue)
	- 父节点获取 node.parentNode
- 孩子节点操作
	- 遍历孩子节点 nodes2Arr + [].map
	- 解析孩子节点 compileNode
 

## 模板事件管理
``` js
	//存储
    var eventPool = {};	
    //监听(也可以称为注册或添加)
    sodaRender.addEventListener = function(type, func) {
        if (eventPool[type]) {} else {
            eventPool[type] = [];
        }

        eventPool[type].push(func);
    };
    //移除(没有)
    //触发(获取)
    var triggerEvent = function(type, e, data) {
        var events = eventPool[type] || [];

        for (var i = 0; i < events.length; i++) {
            var eventFunc = events[i];
            eventFunc && eventFunc(e, data);
        }
    };        
```
# reference
[AlloyTeam/sodajs](https://github.com/AlloyTeam/sodajs)
