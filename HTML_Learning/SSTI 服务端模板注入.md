# SSTI 服务端模板注入

> SSTI(Server-Side Template Injection)服务端模板注入CTF web常见考点

## What is SSTI?

Python的一些Web开发知识，其中的Flask，Django这些MVC框架时，用户输入的变量被接收后通过Controller处理，最后通过渲染返回给了View即HTML页面，而模板引擎处理一些变量时，未经过任何的处理即被编译执行渲染，导致了一些代码执行，信息泄露等。比如使用render_template渲染函数时，未经过处理就对用户输入的变量做了解析变换。

## python的类

### class类

查看变量所属的类：字符，字典，元组，列表

```python
s = [1, 2, 3]
print(s.__class__)
s = 'abc'
print(s.__class__)
s = {'Aiwin': 1}
print(s.__class__)
s = ({'Aiwin': 1}, [1, 2, 3])
print(s.__class__)
 
输出:
<class 'list'>
<class 'str'>
<class 'dict'>
<class 'tuple'>
```

### bases类

