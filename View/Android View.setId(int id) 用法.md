# Android View.setId(int id) 用法
> 当要在代码中动态的添加View并且为其设置id时,如果直接用一个int值时,Studio会警告.
> 经过查询,动态设置id的方法有两种;

### 1. View.generateViewId();
> 这个方法的返回值是个int值,方法的意思是获取一个可以用在`setId(int id)`方法中的int类型id;

[官方文档说明:](https://developer.android.google.cn/reference/android/view/View.html)

> ```
> int generateViewId () 						Added in API level 17
> Generate a value suitable for use in setId(int). This value will not collide with ID values generated at build time by aapt for R.id.
> Returns int  a generated ID value
> ```

**缺点是,你需要用一个变量去记录此id,第二就是它是api17才加入的方法**

### 2.在xml中定义id
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item name="id_add_file" type="id"/>
</resources>
```

使用就很简单:

```
imageView.setId(R.id.id_add_file);
```
