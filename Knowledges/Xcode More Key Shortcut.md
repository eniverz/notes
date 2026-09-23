---
type: software
tags:
  - software
  - system/MacOS
done: true
topic:
  - "[[System]]"
  - "[[Coding]]"
---
# 快捷键设置

所有的快捷键绑定的功能列表在:
```
/Applications/Xcode.app/Contents/Frameworks/IDEKit.framework/Versions/A/Resources/IDETextKeyBindingSet.plist
```

这里面可以自定义快捷键的组合.


在这里面添加一个key, 类型选择Dictionary. 里面可以添加两个Key, `Insert Newline Above` 和 `Insert Newline Below`, 都是string类型的. 里面填充command如下:

![[Pasted image 20250819192014.png]]

将这个文件保存(或者保存在其他位置, 替换回去)

> [!warning]
> 由于权限问题, 不能直接修改. 必须将这个文件复制到别的地方进行修改, 然后在finder中复制回去(会触发一个Auth弹窗, 认证之后就可以覆盖掉原来的文件)

重启Xcode, 然后进入`Preferences -> Key Bindings`里面可以找到新加的这两个功能, 然后绑定key即可
