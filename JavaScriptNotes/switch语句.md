# JavaScript的switch语句

语法:
```JavaScript
switch(n)
{
    case 1:
        // 执行代码块 1
        break;
    case 2:
        // 执行代码块 2
        break;
    default:
        // 与 case 1 和 case 2 不同时执行的代码
}
```

工作原理：首先设置表达式 n（通常是一个变量）。随后表达式的值会与结构中的每个 case 的值做比较。如果存在匹配，则与该 case 关联的代码块会被执行。请使用 break 来阻止代码自动地向下一个 case 运行。


实例:
```JavaScript
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>switch语句</title>
</head>
<body>

<p>点击下面的按钮来显示今天是周几：</p>
<button onclick="myFunction()">点击这里</button>
<p id="demo"></p>
<script>
function myFunction(){
	var x;
	var d=new Date().getDay();
	switch (d){
  		case 0:x="今天是星期日";
    	break;
 		case 1:x="今天是星期一";
        break;
  		case 2:x="今天是星期二";
        break;
        case 3:x="今天是星期三";
   	 	break;
  		case 4:x="今天是星期四";
    	break;
  		case 5:x="今天是星期五";
        break;
  		case 6:x="今天是星期六";
    	break;
 	}
	document.getElementById("demo").innerHTML=x;
}
</script>

</body>
</html>
```
点击按钮输出今天是星期几.

## default 关键词

default 关键词来规定匹配不到条件时候的输出

```JavaScript

<html>
<head>
<meta charset="utf-8">
<title>switch语句</title>
</head>
<body>

<p>点击下面的按钮，会显示出基于今日日期的消息：</p>
<button onclick="myFunction()">点击这里</button>
<p id="demo"></p>
<script>
function myFunction()
{
	var x;
	var d=new Date().getDay();
	switch (d)
    {
  		case 6:x="今天是星期六";
    	break;
  		case 0:x="今天是星期日";
    	break;
  		default:
    	x="期待周末";
  	}
	document.getElementById("demo").innerHTML=x;
}
</script>

</body>
</html>
```

如果今天不是星期六或星期日，则会输出默认的消息.

