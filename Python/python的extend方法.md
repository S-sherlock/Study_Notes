在开发中，列表操作是常有的事。

免不了的把东西从一个列表中放到另外一个列表中

一般的思路是这样：

```python
list_1 = [1,2,3,4]
list_2 = ["A","b"]

for i in list_1:
    list_2.append(i)

print(list_2)
# 输出是：['A', 'b', 1, 2, 3, 4]
```

用extend方法：
```python
list_1 = [1,2,3,4]
list_2 = ["A","b"]

list_2.extend(list_1)

print(list_2)
```
其输出的结果是同样的。但是少写了一行。看着比较简单，符合python的风格。
而且在开发中多用python自己的内置函数，也会节省内存一些。

**`extend`的参数接收一个可迭代的对象**
也就是说`extend`的参数必须是可迭代的，比如元祖，列表，字典，甚至字符串

接收字典：

```python
dict_1 = {"A":1, "b":2}
list_2 = ["A","b"]

list_2.extend(dict_1)
print(list_2)
# 输出：['A', 'b', 'A', 'b']
list_2.extend(dict_1.values())
print(list_2)
# 输出：['A', 'b', 1, 2]
```
字典默认迭代的是`key`。

接收元组：
```python
tuple_1 = ((1,2), (2,3))
list_2 = ["A","b"]

list_2.extend(tuple_1)
print(list_2)
# 输出：['A', 'b', (1, 2), (2, 3)]
```

接收字符串：

```python
str_1 = 'python'
list_2 = ["A","b"]

list_2.extend(str_1)
print(list_2)
# 输出：['A', 'b', 'p', 'y', 't', 'h', 'o', 'n']
```

---
以上就是`python`内置函数`extend`的用法，今天也是突然看到这个方法，觉得比`for`循环来的简洁。所以记录下来.