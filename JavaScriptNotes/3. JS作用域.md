# JS的作用域

## JS的局部作用域
变量在函数内声明，变量为局部作用域。

局部变量：只能在函数内部访问。

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>局部作用域</title> 
</head>
<body>

<p>局部变量在声明的函数外不可以访问。</p>
<p id="demo"></p>
<script>
myFunction();
document.getElementById("demo").innerHTML = "carName 的类型是：" +  typeof carName;
function myFunction() 
{
    var carName = "Volvo";
}
</script>

</body>
</html>
```
运行结果:
```html
局部变量在声明的函数外不可以访问。

carName 的类型是：undefined
```
因为局部变量只作用于函数内，所以不同的函数可以使用相同名称的变量。

局部变量在函数开始执行时创建，函数执行完后局部变量会自动销毁。

## JS全局变量

变量在函数外定义，即为全局变量。

全局变量有 **全局作用域**: 网页中所有脚本和函数均可使用。
```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>全局变量</title> 
</head>
<body>

<p>全局变量在任何脚本和函数内均可访问。</p>
<p id="demo"></p>
<script>
var carName = "Volvo";
myFunction();
function myFunction() 
{
    document.getElementById("demo").innerHTML =
		"我可以显示 " + carName;
}
</script>

</body>
</html>
```
```html
全局变量在任何脚本和函数内均可访问。

我可以显示 Volvo
```

如果变量在函数内没有声明（没有使用 var关键字），该变量为全局变量。

以下实例中 carName 在函数内，但是为全局变量。

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>全局变量</title> 
</head>
<body>

<p>
如果你的变量没有声明，它将自动成为全局变量：
</p>
<p id="demo"></p>
<script>
myFunction();
document.getElementById("demo").innerHTML =
	"我可以显示 " + carName;
function myFunction() 
{
    carName = "Volvo";
}
</script>

</body>
</html>
```

```html
如果你的变量没有声明，它将自动成为全局变量：

我可以显示 Volvo
```
**JavaScript 变量生命周期**

- JavaScript 变量生命周期在它声明时初始化。

- 局部变量在函数执行完毕后销毁。

- 全局变量在页面关闭后销毁。

## HTML 中的全局变量
在 HTML 中, 全局变量是 window 对象: 所有数据变量都属于 window 对象。
```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>HTML 全局变量</title> 
</head>
<body>

<p>
在 HTML 中, 所有全局变量都会成为 window 变量。
</p>
<p id="demo"></p>
<script>
myFunction();
document.getElementById("demo").innerHTML =
	"我可以显示 " + window.carName;
function myFunction() 
{
    carName = "Volvo";
}
</script>

</body>
</html>
```
```html
在 HTML 中, 所有全局变量都会成为 window 变量。

我可以显示 Volvo
```
