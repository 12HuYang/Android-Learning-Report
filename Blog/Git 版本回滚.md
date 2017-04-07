# Git 版本回滚

> 刚刚遇到一次，虽然以前也遇到过多次，但是对命令不熟，每次都要Google一下，索性记下来！

## 强制将`HEAD`指向某次`commit`

```
git reset --hard 200c2832e73c0c29262fd5711fd89776abe23f87
```

`soft` 和 `hard` 参数的区别:
1. `hard`修改记录都没了
2. `soft`则会保留修改记录
3. 在出现异常，比如merge错误等无法使用`soft`

## 强制`push`覆盖

```
git push -f
```

## 总结

就两个命令：

```
1. git reset  回滚到某个版本
2. git push -f  强制push覆盖
```