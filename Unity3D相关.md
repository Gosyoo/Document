# 记录

## 关于代码中使用C#  dynamic关键字

报错提示：

```
Missing compiler required member 'Microsoft.CSharp.RuntimeBinder.CSharpArgumentInfo.Create'
```

解决方案：

通过选择**Edit > Project Settings > Player > Other Settings**来打开 Unity Inspector 中的 **PlayerSettings**

在configuration 中 选择 Api compatibility Level 选中**.Net 4x** 或 **.Net Framework**

等待编辑器刷新，即可解决

[官方文档](https://learn.microsoft.com/en-us/visualstudio/gamedev/unity/unity-scripting-upgrade)

