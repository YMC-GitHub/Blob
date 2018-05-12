# Partials

## Basic Partials

为了使用partial,必须先用接口注册。
```js
hbs.registerPartial('partial_name','{{partial_cotent}}');
```
> Partials必须被编译才能使用，注册的时候第二个参数就是预编译时需要的模板。

现在使用partial：
```html
{{> partial_name}}
```

## Dynamic Partials

可以使用子表达式动态使用一个partial。
```html
{{> (which partial_name)}}
```
> 子表达式不能是变量，所以只能是一个函数。

如果一个变量的取值是partial的名字，可以通过lookup helper动态使用：
```html
{{> (lookup .'variant_name')}}
```

## Partial Context

使用partial的时候，传入上下文，可以实现自定义执行partial的上下文。
```html
{{> partial_name partial_context}}
```

## Partial Parameters
通过hash参数，自定义数据可以传入partial。
```html
{{> partial_name parameter_name=parameter_value}}
```

可以使用相对路径访问父级变量:
```html
{{> partial_name name=../parent_name}}
```

> 相当于vue中父组件向子组件传数据。

## Partial Blocks

当尝试渲染一个找不存在的partial时，正常行为是通过接口抛出一个错误。
使用partial时，通过块级方式引用，代替抛出一个错误：
```html
{{#> partial_name}}
 i will be show when this partial not found
{{/partial_name}}
```

块级方式还可以把template传给partial。
```html
{{<!--a template of-->}}
{{#> partial_name}}
 i will be show when this partial not found
{{/partial_name}}

{{<!--a partial of-->}}
Site Content
{{< @partial-block}}

{{<!--will render-->}}

```

## Inline Partials

通过inline声明符，templates 可以定义块级作用域 partials。
```html
{{<!--a template of-->}}
{{#*inline "partial_name"}}
 i will be show when this if is false
{{/inline}}

{{#each children}}
	{{> partial_name}}
{{/each}}

```

假如有一个叫做layout的partial：
```html
<div class="header">
	{{> header}}
<div>
<div class="body">
	{{> body}}
<div>
<div class="footer">
	{{> footer}}
<div>
```

这样使用这个layput的partial会好一点：
```html
{{#> layput}}
	{{#*inline "header"}}
 		i will be show when this if is false
	{{/inline}}
	{{#*inline "footer"}}
 		i will be show when this if is false
	{{/inline}}
	{{#*inline "footer"}}
 		i will be show when this if is false
	{{/inline}}	
{{/layput}}
```


# Built-In Helpers

## The if block helper
根据条件进行渲染一个block，可以使用if helper。
如果它的参数返回值是false,undefined,null,'',0或者是[],则不会渲染这个block。
```html
{{#if resume}}
	<h1>{{title}}</h1>
{{/if}}
```
如果使用一个空的上下文(resume为{}),resume会被当成是undefined。

使用block expression时，可以指定一个template section，当参数返回值是false时。这个section使用{{else}}标记。
```html
{{#if resume}}
	<h1>{{title}}</h1>
{{else}}	
	<h1>i will be show if resume returning false</h1>
{{/if}}
```

## The unless block helper
可以使用unless helper,起到与if helper相反的作用。这个block会被渲染，如果它的expression返回值是false。
```html
{{#unless resume}}
	<h1>waring: this entry does not have a resume</h1>
{{else}}	
	
{{/unless}}
```

## The each block helper
可以遍历一个列表，使用 each helper。在这个block内，可以使用this引用正在遍历的元素。
```html
{{#each resume_part}}
	<h1>waring: this entry does not have a resume</h1>	
	
{{/each}}
```
可以使用this expression在任一的上下文，引用当前的上下文。
可以选择性地使用{{else}} setion,显示一些内容，当列表为空时。
```html
{{#each resume_part}}
	<h1>{{this}}</h1>	
{{else}}
	<h1>waring: this entry does not have a resume_part</h1>	
{{/each}}
```

当遍历的对象是一个数组时，可以选择性地引用当前索引，通过{{@index}}。
```html
{{#each resume_part_arr}}
	<h1>{{@index}} : {{this}}</h1>	
{{else}}
	<h1>waring: this entry does not have a resume_part</h1>	
{{/each}}
```

当遍历的对象是一个对象时，可以选择性地引用当前键名，通过{{@key}}。
```html
{{#each resume_part}}
	<h1>{{@key}: {{this}}</h1>	
{{else}}
	<h1>waring: this entry does not have a resume_part</h1>	
{{/each}}
```

当遍历的对象是一个数组时，可以使用@fist/@last引用第一个/最后一个iteration。
当遍历的对象是一个对象时，仅可以使用@fist。

each block 镶嵌时,可以访问父级的索引值，@../index。
```html
{{#each resume_part_arr}}
	<h1>{{@../index}}:{{@index}} : {{this}}</h1>	
{{else}}
	<h1>waring: this entry does not have a resume_part</h1>	
{{/each}}
```

另外，each helper还支持block parameters,允许别名引用在这个block内。
```html
{{#each resume_part_arr as |val key|}}
	{{#each child as |child_val child_key|}}
		<h1>{{key}}:{{child_key}} : {{child_val}}</h1>	
	{{else}}
	{{/each}}	
{{else}}
{{/each}}
```


## The with block helper
正常情况下，会使用传入的上下文进行编译模板。
```js

```

```html
{{#with resume_part_obj}}
		<h1>{{key1}} {{key2}} {{key3}}</h1>	
	{{/each}}	
{{else}}
{{/with}}
```

别名
```html
{{#with resume_part_obj as |part|}}
		<h1>{{part.key}}</h1>	
	{{/each}}	
{{else}}
{{/with}}
```

## The lookup block helper
lookup helper 允许动态参数引用。

## The log block helper
log helper 允许访问上下文状态，当执行模板时。

## The blockHelperMissing helper

## The helperMissing helper

## reference
[handlebars-official-tutorial](http://handlebarsjs.com/)
